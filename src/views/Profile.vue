<template>
  <div class="container" style="font-family: 'Montserrat', sans-serif;">
    <div class="card mt-3" style="align-items:center;border:none;">
        <div v-if="isUserProfile" @click="$refs.profileImage.click()" style="cursor:pointer;">
            <b-avatar variant="info" :src="profilePicture" size="10rem"></b-avatar>
        </div>
        <div v-else>
            <b-avatar variant="info" :src="profilePicture" size="10rem"></b-avatar>
        </div>
        <input type="file" ref="profileImage" style="display: none" @change="updateProfilePicture">
        <span class="mt-2"><h4>{{ profile.username }}</h4></span>
        <div style="display:flex;margin-bottom:20px;">
            <div class="mr-2" style="text-align:center;">
                <p>Posts</p>
                <span v-if="profile.images" class="mt-0"><b>{{profile.images.length}}</b></span>
            </div>
            <div class="mr-2" style="text-align:center;">
                <p>Followers</p>
                <span><b>{{ profile.followers }}</b></span>
            </div>
            <div style="text-align:center;">
                <p>Following</p>
                <span><b>{{ profile.following }}</b></span>
            </div>
        </div>
        <input type="file" ref="image" style="display: none" @change="addImage">
        <label v-if="isUserProfile" class="formLabel" @click="$refs.image.click()" for="input-file">
            <div>+</div>
        </label>
        <button v-if="isFollowing" class="btn btn-outline-primary">Unfollow</button>
        <button v-if="!isFollowing" class="btn btn-outline-primary" @click="follow()">Follow</button>
    </div>
    <Gallery :images="profile.images" :profilePicture="profilePicture" :profile="profile"/>
  </div>
</template>

<script>
import Gallery from '../components/Gallery.vue'
import { profileService } from '../services/ProfileService'
import { mapGetters } from 'vuex'

export default {

    data() {
        return {
            profile: {},
            isFollowing: null
        }
    },

    components: {
        Gallery
    },

    computed:{
        ...mapGetters({
            userId: 'getUserId',
            userProfileId: 'getUserProfileId'
        }),
        profilePicture() {
            return this.profile.image ? this.profile.image : '../assets/default-profile-pic.jpg';
        },
        isUserProfile() {
            return !this.$route.params.profile_id;
        }
    },

    methods: {
        updateProfilePicture(event) {
            let profileImage = event.target.files[0];
            let fd = new FormData();
            fd.set('profile_image', profileImage)
            fd.set('user_id', this.userId)
            profileService.updateProfilePicture(fd).then(response => {
                this.profile = response.data.profile
            })
        },
        addImage(event) {
            let image = event.target.files[0];
            let fd = new FormData();
            fd.set('image', image)
            fd.set('user_id', this.userId)
            profileService.addImage(fd).then(response => {
                this.profile.images = response.data.images
            })
        },

        follow() {
            let followData = {};
            followData.profile_id = this.profile.id
            followData.user_profile_id = this.userProfileId
            profileService.follow(followData).then(response => {
                this.profile = response.data.profile;
                this.isFollowing = true
            })
        }
    },

    created() {
        if (!this.$route.params.profile_id) {
            profileService.getProfileByUser(this.userId).then(r => {
                this.profile = r.data.profile;
            })
            return;
        }
        let followData = {};
        followData.profile_id = this.$route.params.profile_id
        followData.user_profile_id = this.userProfileId

        profileService.isFollowing(followData).then(response => {
            this.isFollowing = response.data.isFollowing;
        })
        profileService.getProfile(this.$route.params.profile_id).then(r => {
            this.profile = r.data.profile;
        })
    }
}
</script>

<style>
.formLabel {
    color: #ffb366;
    font-size: 30px;
    width: 40px;
    height: 40px;
    border: 2px solid grey;
    border-radius: 50%;
    cursor: pointer;
    background-color: white;
    margin-top: 10px;
    margin-bottom: 10px;
    text-align: center;
}
</style>