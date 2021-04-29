<template>
  <q-layout view="lHh Lpr lFf">
    <q-header
      class="bg-white text-grey-10"
      bordered
    >
      <q-toolbar class="constrain">
        <q-btn
          class="large-sceen-only q-mr-sm"
          icon="eva-camera-outline"
          flat size="18px"
          round
          dense
          to="/camera"/>
        <q-separator
          class="large-sceen-only"
          vertical
          spaced
        />
        <q-toolbar-title class="text-gram-hotel text-bold">
          Nerdgram
        </q-toolbar-title>
        <q-btn
          class="large-sceen-only"
          icon="eva-home-outline"
          flat size="18px"
          round
          dense
          to="/"/>
      </q-toolbar>
    </q-header>
    <q-footer class="bg-white small-sceen-only" bordered>
      <q-tabs class="text-grey-10" active-color="primary" indicator-color="transparent">
        <q-route-tab to="/" icon="eva-home-outline" />
        <q-route-tab to="/camera" icon="eva-camera-outline" />
      </q-tabs>
    </q-footer>
    <q-page-container class="bg-grey-1">
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>

export default {
  name: 'MainLayout',
  data() {
    return {

    };
  },
};
</script>

<style lang="sass">
  .q-footer
    .q-tab__icon
      font-size: 30px
  .q-toolbar__title
    @media( max-width: $breakpoint-xs-max)
      text-align: center
    font-size: 30px
  .q-toolbar
    @media( min-width: $breakpoint-sm-min)
      height: 77px
</style>
