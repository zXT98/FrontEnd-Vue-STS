<template>
  <v-app>
    <v-app-bar app color="primary" dark dense>
      <v-spacer></v-spacer>
    </v-app-bar>

    <v-navigation-drawer app clipped >
      <v-list-item to="/">
        <v-list-item-content>
          <v-list-item-title>
            Home 
          </v-list-item-title>
        </v-list-item-content>
      </v-list-item>
      <v-list-item to="/discovery">
        <v-list-item-content>
          <v-list-item-title>
            Discovery
          </v-list-item-title>
        </v-list-item-content>
      </v-list-item>
      <v-list-item to="/scan">
        <v-list-item-content>
          <v-list-item-title>
            Scan
          </v-list-item-title>
        </v-list-item-content>
      </v-list-item>
      <v-list-item to="/commands">
        <v-list-item-content>
          <v-list-item-title>
            Commands
          </v-list-item-title>
        </v-list-item-content>
      </v-list-item>
    </v-navigation-drawer>

    <v-main>
      <v-container>
        <router-view />
      </v-container>
    </v-main>
  </v-app>
</template>

<script>

export default {
  name: 'App',
  components: {
  },

  data: () => ({
    //
  }),
};
</script>
