<script>
import ModalStore from '../../stores/modal_store';
import modalMixin from '../../mixins/modal_mixins';

export default {
  mixins: [modalMixin],
  data() {
    return ModalStore.store;
  },
  computed: {
    selectedCount() {
      return ModalStore.selectedCount();
    },
  },
  destroyed() {
    this.activeTab = 'all';
  },
};
</script>
<template>
  <div class="top-area prepend-top-10 append-bottom-10">
    <ul class="nav-links issues-state-filters">
      <li :class="{ active: activeTab == 'all' }">
        <a href="#" role="button" @click.prevent="changeTab('all');">
          Open issues <span class="badge badge-pill"> {{ issuesCount }} </span>
        </a>
      </li>
      <li :class="{ active: activeTab == 'selected' }">
        <a href="#" role="button" @click.prevent="changeTab('selected');">
          Selected issues <span class="badge badge-pill"> {{ selectedCount }} </span>
        </a>
      </li>
    </ul>
  </div>
</template>
