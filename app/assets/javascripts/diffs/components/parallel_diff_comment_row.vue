<script>
import diffDiscussions from './diff_discussions.vue';
import diffLineNoteForm from './diff_line_note_form.vue';

export default {
  components: {
    diffDiscussions,
    diffLineNoteForm,
  },
  props: {
    line: {
      type: Object,
      required: true,
    },
    diffFileHash: {
      type: String,
      required: true,
    },
    lineIndex: {
      type: Number,
      required: true,
    },
  },
  computed: {
    hasExpandedDiscussionOnLeft() {
      return this.line.left && this.line.left.discussions.length
        ? this.line.left.discussions.every(discussion => discussion.expanded)
        : false;
    },
    hasExpandedDiscussionOnRight() {
      return this.line.right && this.line.right.discussions.length
        ? this.line.right.discussions.every(discussion => discussion.expanded)
        : false;
    },
    hasAnyExpandedDiscussion() {
      return this.hasExpandedDiscussionOnLeft || this.hasExpandedDiscussionOnRight;
    },
    shouldRenderDiscussionsOnLeft() {
      return this.line.left && this.line.left.discussions && this.hasExpandedDiscussionOnLeft;
    },
    shouldRenderDiscussionsOnRight() {
      return (
        this.line.right &&
        this.line.right.discussions &&
        this.hasExpandedDiscussionOnRight &&
        this.line.right.type
      );
    },
    showRightSideCommentForm() {
      return this.line.right && this.line.right.type && this.line.right.hasForm;
    },
    showLeftSideCommentForm() {
      return this.line.left && this.line.left.hasForm;
    },
    className() {
      return (this.left && this.line.left.discussions.length > 0) ||
        (this.right && this.line.right.discussions.length > 0)
        ? ''
        : 'js-temp-notes-holder';
    },
    shouldRender() {
      const { line } = this;
      const hasDiscussion =
        (line.left && line.left.discussions && line.left.discussions.length) ||
        (line.right && line.right.discussions && line.right.discussions.length);

      if (
        hasDiscussion &&
        (this.hasExpandedDiscussionOnLeft || this.hasExpandedDiscussionOnRight)
      ) {
        return true;
      }

      const hasCommentFormOnLeft = line.left && line.left.hasForm;
      const hasCommentFormOnRight = line.right && line.right.hasForm;

      return hasCommentFormOnLeft || hasCommentFormOnRight;
    },
  },
};
</script>

<template>
  <tr v-if="shouldRender" :class="className" class="notes_holder">
    <td class="notes_content parallel old" colspan="2">
      <div v-if="shouldRenderDiscussionsOnLeft" class="content">
        <diff-discussions
          v-if="line.left.discussions.length"
          :discussions="line.left.discussions"
        />
      </div>
      <diff-line-note-form
        v-if="showLeftSideCommentForm"
        :diff-file-hash="diffFileHash"
        :line="line.left"
        :note-target-line="line.left"
        line-position="left"
      />
    </td>
    <td class="notes_content parallel new" colspan="2">
      <div v-if="shouldRenderDiscussionsOnRight" class="content">
        <diff-discussions
          v-if="line.right.discussions.length"
          :discussions="line.right.discussions"
        />
      </div>
      <diff-line-note-form
        v-if="showRightSideCommentForm"
        :diff-file-hash="diffFileHash"
        :line="line.right"
        :note-target-line="line.right"
        line-position="right"
      />
    </td>
  </tr>
</template>
