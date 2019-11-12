<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated>
      <q-toolbar>
        <q-btn
          flat
          dense
          round
          @click="leftDrawerOpen = !leftDrawerOpen"
          icon="menu"
          aria-label="Menu"
        />

        <q-toolbar-title>
          Rapido y Facil
        </q-toolbar-title>

      </q-toolbar>
    </q-header>

    <q-drawer
      v-model="leftDrawerOpen"
      show-if-above
      bordered
      content-class="bg-grey-2"
    >
      <q-list>
        <q-item clickable tag="a" href="">
          <q-item-section avatar>
            <q-icon name="home" />
          </q-item-section>
          <q-item-section>
            <q-item-label>Home</q-item-label>
            <q-item-label caption>/</q-item-label>
          </q-item-section>
        </q-item>
        <q-item clickable tag="a" href="">
          <q-item-section avatar>
            <q-icon name="user" />
          </q-item-section>
          <q-item-section>
            <q-item-label>Sobre Nosotros</q-item-label>
            <q-item-label caption>/nosotros</q-item-label>
          </q-item-section>
        </q-item>
        <q-item clickable tag="a" href="">
          <q-item-section avatar>
            <q-icon name="user" />
          </q-item-section>
          <q-item-section>
            <q-item-label>Productos</q-item-label>
            <q-item-label caption>/productos</q-item-label>
          </q-item-section>
        </q-item>
      </q-list>
    </q-drawer>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>
export default {
  name: 'MyLayout',

  data () {
    return {
      leftDrawerOpen: false
    }
  }
}
</script>
