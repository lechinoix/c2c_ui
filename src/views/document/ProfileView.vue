<template>
  <div class="section has-background-white-print">
    <loading-notification :promise="promise" />
    <document-view-header v-if="document" :document="document" :version="version" />

    <div v-if="document && document.not_authorized" v-translate class="notification is-danger">
      This profile is only available to authenticated users.
    </div>

    <div v-if="document && !document.not_authorized" class="columns">
      <div class="column is-3">
        <div class="box">
          <activities-field v-if="document.activities && document.activities.length" :document="document" />

          <label-value :label="$gettext('forum')">
            <a :href="$options.forumUrl + '/users/' + document.forum_username + '/activity'">
              @{{ document.forum_username }}
            </a>
          </label-value>

          <field-view :document="document" :field="fields.categories" />

          <div class="buttons is-centered button-outings">
            <outings-downloader :profile-id="documentId" />
          </div>
        </div>

        <map-box :document="document" v-if="document.geometry && document.geometry.geom" />
        <tool-box :document="document" />
      </div>

      <div class="column is-9">
        <div class="box" v-if="locale.summary || locale.description">
          <markdown-section :document="document" :field="fields.summary" />
          <markdown-section :document="document" :field="fields.description" hide-title />
          <div style="clear: both" />
        </div>

        <images-box :document="document" />

        <feed-widget type="profile" />
      </div>
    </div>
  </div>
</template>

<script>
import OutingsDownloader from './utils/OutingsDownloader';
import documentViewMixin from './utils/document-view-mixin';

import FeedWidget from '@/components/feed-widget/FeedWidget';
import config from '@/js/config';

export default {
  components: {
    FeedWidget,
    OutingsDownloader,
  },

  mixins: [documentViewMixin],

  forumUrl: config.urls.forum,
};
</script>

<style lang="scss" scoped>
.button-outings {
  margin-top: 1rem;
}
</style>
