<template lang="pug">
  b-modal#remove-member(:title="$t('removeMember')", size='md', :hide-footer="true")
    .text-center
      h2.col-12 {{ $t('sureKick') }}
      .col-12.removing-member(v-if='memberToRemove.profile') {{memberToRemove.profile.name}}
    .modal-body
      textarea.form-control(type='text',
        rows='5',
        :placeholder="$t('optionalMessage')",
        v-model='removeMessage')
    .modal-footer
      button.pull-left.btn.btn-danger(@click='confirmRemoveMember()') {{ $t('yesRemove') }}
      button.btn.btn-default(@click='close()') {{ $t('cancel') }}
</template>

<style scoped>
  .removing-member {
    color: #878190;
    margin-bottom: .5em;
  }
</style>

<script>
import bModal from 'bootstrap-vue/lib/components/modal';

export default {
  props: ['memberToRemove', 'groupId'],
  components: {
    bModal,
  },
  data () {
    return {
      removeMessage: '',
    };
  },
  methods: {
    async confirmRemoveMember () {
      await this.$store.dispatch('members:removeMember', {
        memberId: this.memberToRemove._id,
        groupId: this.groupId,
        message: this.removeMessage,
      });

      this.removeMessage = '';
      this.$emit('member-removed', this.memberToRemove);
      this.close();
    },
    close () {
      this.$root.$emit('hide::modal', 'remove-member');
    },
  },
};
</script>
