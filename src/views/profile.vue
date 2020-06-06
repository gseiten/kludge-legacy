<template>

<div class="main-container">

	<article class="media profile-banner">

		<figure class="media-left">
			<p class="image is-128x128" >
				<img v-if="loggedInUser.thumbnail" :src="loggedInUser.thumbnail">
				<img v-else :src="profilepicture">
			</p>
			<b-upload v-model="profilepicture_file" class="editprofilepic__button" @input="updateProfilePicture">
				<div class="button is-dark">
					<i class="fas fa-edit"></i>	
				</div>
			</b-upload>
		</figure>

		<div class="media-content">
			<div class="content">
				<div class="columns">
					<div class="column is-6">
						<div class="is-size-3">
							{{ loggedInUser.username }}
						</div>
						<p class="bio is-size-6">Lorem ipsum dolor sit amet, consectetur adipisicing elit. Est modi perspiciatis ipsum? Eos fugiat consequuntur unde sit dolores veritatis.</p>
					</div>
					<div class='column is-2-tablet is-4-mobile has-text-centered'>
						<p class='stat-val'>30</p>
						<p class='stat-key'>Successful <br> Challenges</p>
					</div>
					<div class='column is-2-tablet is-4-mobile has-text-centered'>
						<p class='stat-val'>10</p>
						<p class='stat-key'>Articles <br> Written</p>
					</div>
					<div class='column is-2-tablet is-4-mobile has-text-centered'>
						<p class='stat-val'>50</p>
						<p class='stat-key'>Accepted <br> Answers</p>
					</div>
				</div>
			</div>
		</div>

	</article>

	<b-tabs type="" size="" v-model="activeTab" :multiline="true">

		<b-tab-item>
			<template slot="header">
				<span>Your Projects</span>
			</template>
			<div class="list profile-list">
				<div class="list-item" v-for="(item, index) in your_projects" :key="index">
					{{ item }}
				</div>
			</div>
		</b-tab-item>

		<b-tab-item>
			<template slot="header">
				<span>Saved Articles</span>
			</template>
			<div class="list profile-list">
				<div class="list-item" v-for="(item, index) in saved_articles" :key="index">
					{{ item }}
				</div>
			</div>
		</b-tab-item>

		<b-tab-item>
			<template slot="header">
				<span>Questions Asked</span>
			</template>
			<div class="list profile-list">
				<div class="list-item" v-for="(item, index) in questions_asked" :key="index">
					{{ item }}
				</div>
			</div>
		</b-tab-item>

		<b-tab-item>
			<template slot="header">
				<span>Active Challenges</span>
			</template>
			<div class="list profile-list">
				<div class="list-item" v-for="(item, index) in active_challenges" :key="index">
					{{ item }}
				</div>
			</div>
		</b-tab-item>

	</b-tabs>

</div>


</template>

<script>

import axios from 'axios';

export default {
	name: "profile",
	components: {},
	data() {
	return {
			loggedInUser: this.$store.getters.loggedInUser,
			activeTab: 0,
			your_projects: [],
			saved_articles: [],
			questions_asked: [],
			active_challenges: [],
			profilepicture: 'https://api.adorable.io/avatars/2',
			profilepicture_file: null,
		}
	},
	methods: {

		updateProfilePicture(file){
			const reader = new FileReader();
			reader.addEventListener('load', (result) => {
				this.profilepicture = result.target.result;
			});
			reader.readAsDataURL(file);
		}

	},

	created(){

		axios.get('http://api.icndb.com/jokes/random/50').then(response => {
			let jokes = [];
			response.data.value.forEach(element => {
				jokes.push(element.joke)
			});
			this.your_projects = this.saved_articles = this.questions_asked = this.active_challenges = jokes;
		}).catch(error => {
			this.$buefy.snackbar.open({message: error, type: 'is-danger'})
		}).then(() => {});
	
	}
};

</script>

<style lang="css" scoped>



.profile-banner .media-left {
	position: relative;
}

.editprofilepic__button{
	font-size: 13px;
	position: absolute;
	bottom: 0;
	right: 0;
	display: none;
}

.editprofilepic__button:hover{
	display: block;
}

.profile-banner .media-left .image:hover + .editprofilepic__button{
	display: block;
}


.main-container {
	height: 100%;
	margin-right: 12px;
	padding-bottom: 2em;
}

::v-deep * {
	color: whitesmoke;
}

::v-deep .b-tabs .tab-content{
	padding: 0;
}

::v-deep .b-tabs .tabs ul{
	background-color: hsl(0, 0%, 7%)
}

::v-deep .b-tabs .tabs li.is-active span{
    color: whitesmoke;
}
::v-deep .b-tabs .tabs li.is-active a{
    color: whitesmoke;
	border-bottom-color: yellow	;
}


/* deep because "columns" from "home" was messing with columns of this components. "min-height" specifically */
::v-deep .profile-banner .media-content .content .columns{
	min-height: 100%
}


.profile-banner{
	/* background-color: hsl(0, 0%, 7%); */
	height: 170px;
	padding: 15px;
	overflow-y: auto;
}

.stat-val {
	font-size: 2.5em;
	font-weight: bold;
	margin-bottom: 0 !important;
}

.stat-key {
	font-size: 1.2em;
	font-weight: 200
}

.profile-list{
	border-radius: 0%;
	background-color: hsl(0, 0%, 10%);
}

.profile-list .list-item {
	color: whitesmoke;
	border-bottom-color: hsl(0, 0%, 19%);
}

.profile-list .list-item:hover {
	background-color: hsl(0, 0%, 14%);
}




</style>