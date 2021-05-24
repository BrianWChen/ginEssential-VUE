<template>
  <div>
    <b-navbar
      toggleable="lg"
      type="dark"
      variant="info"
    >
      <b-container>
        <b-navbar-brand @click="$router.push({name: 'Home'})">Logo</b-navbar-brand>

        <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

        <b-collapse
          id="nav-collapse"
          is-nav
        >
          <!-- <b-navbar-nav>
        <b-nav-item href="#">Link</b-nav-item>
        <b-nav-item href="#" disabled>Disabled</b-nav-item>
    </b-navbar-nav> -->

          <!-- Right aligned nav items -->
          <b-navbar-nav class="ml-auto">
            <!-- <b-nav-form>
        <b-form-input size="sm" class="mr-sm-2" placeholder="Search"></b-form-input>
        <b-button size="sm" class="my-2 my-sm-0" type="submit">Search</b-button>
        </b-nav-form> -->

            <b-nav-item-dropdown
              right
              v-if="userInfo"
            >
              <template #button-content>
                <em>{{userInfo.name}}</em>
              </template>
              <b-dropdown-item href="#">My Page</b-dropdown-item>
              <b-dropdown-item @click='logout'>Sign Out</b-dropdown-item>
            </b-nav-item-dropdown>
            <div v-if="!userInfo">
              <b-nav-item
                @click="$router.replace({name: 'Login'})"
                v-if="$router.name !== 'Login'"
              >sign in</b-nav-item>
              <b-nav-item
                @click="$router.replace({name: 'Register'})"
                v-if="$router.name !== 'Register'"
              >sign up</b-nav-item>
            </div>
          </b-navbar-nav>
        </b-collapse>
      </b-container>
    </b-navbar>
  </div>
</template>

<script>
import { mapState, mapActions } from 'vuex';

export default {
  computed: mapState({
    userInfo: (state) => state.userModule.userInfo,
  }),

  methods: mapActions('userModule', ['logout']),
};
</script>

<style>
</style>
