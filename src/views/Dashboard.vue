<template>
  <div class="container" style="margin-top:50px;text-align: center;">
    <div class="row" style="align-items: center;">
      <div class="col-md-2 mt-3" v-for="profile in profiles" :key="profile.id" style="padding-right: 0;padding-left: 0; align-items: center;">
        <div style="text-align: center;">
           <b-avatar variant="info" :src="profile.image" size="10rem"></b-avatar>
           <div style="font-family: 'Montserrat', sans-serif;text-align: center;margin-top:10px;">
             <span style="cursor:pointer;" @click="goToProfile(profile.id)"><b>{{ profile.username }}</b></span>
             <p>Followers: <b>{{ profile.followers }}</b> Following: <b>{{ profile.following}}</b></p>
           </div>
        </div>
        <!-- <div class="card ratio ratio-1x1" style="align-items:center;">
          <img class="card-img-top" style="display: block; cursor: pointer; border-radius: 500px;" @click="openImage(image)" :src="profile.image" data-holder-rendered="true">
        </div> -->
      </div>
    </div>
  </div>
</template>

<script>
import { profileService } from '../services/ProfileService'

export default {
  name: 'Dashboard',

  data() {
    return {
      profiles: []
    }
  },

  methods: {
    goToProfile(profileId) {
      this.$router.push({name: 'Profile', params: {profile_id: profileId}})
    }
  },

  watch: {
    '$route.params.search_term': function (search_term) {
      profileService.getPopularProfiles(search_term).then(response => {
        this.profiles = response.data.profiles
      })
    }
  },

  created() {
    profileService.getPopularProfiles(this.$route.params.search_term, localStorage.getItem('userProfileId')).then(response => {
      this.profiles = response.data.profiles
    })
  }
}
</script>
