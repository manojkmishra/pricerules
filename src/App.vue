<template>
  <v-app id="inspire">
     <v-navigation-drawer 
     v-model="drawer"
     app>
      <v-list-item>
        <v-list-item-content>
          <v-list-item-title class="title">Pricing Rules</v-list-item-title>
        </v-list-item-content>
      </v-list-item>

      <v-divider></v-divider>

      <v-list
        dense
        nav
      >
        <v-list-item v-for="x in items"  :key="x.title"  :to="x.to" link>
            <v-list-item-icon>  <v-icon>{{ x.icon }}</v-icon> </v-list-item-icon>
            <v-list-item-content>
            <v-list-item-title>{{ x.title }}</v-list-item-title>
            </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
    <v-app-bar
      app
      color="primary"
      dark     
    >
      <template v-slot:img="{ props }">
        <v-img
          v-bind="props"
          gradient="to top right, rgba(100,115,201,.7), rgba(25,32,72,.7)"
        ></v-img>
      </template>

      <v-app-bar-nav-icon @click="drawer=!drawer"></v-app-bar-nav-icon>

      <v-app-bar-title>Select Customer and then add Products</v-app-bar-title>

      <v-spacer></v-spacer>

      <v-btn icon>
        <v-icon>mdi-magnify</v-icon>
      </v-btn>


      <v-btn icon>
        <v-icon>mdi-dots-vertical</v-icon>
      </v-btn>


    </v-app-bar>
    <v-main>
       <router-view></router-view>
    </v-main>
  </v-app>
</template>

<script>
  export default {
    data: () => ({ drawer: null,
    items: [
          { title: 'Price List', icon: 'mdi-format-list-checks',to:'/' },
          { title: 'User Guide', icon: 'mdi-help-box', to:'/about' },
          // { title: 'cccc', icon: 'mdi-help-box', to:'/about' },
        ],
        }),
  }
</script>