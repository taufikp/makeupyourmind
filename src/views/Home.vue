<template>
	<main>
		<section v-if="userData.length > 0">
			<div v-for="(user, idx) in userData" :key="idx">
				<span>{{ user.name }}</span>, <span>{{ user.email }}</span><br/>
			</div>
		</section>
		<section v-if="userData2.length > 0">
			<div v-for="(user2, idx2) in userData2" :key="idx2">
				<span>{{ user2.name }}</span>, <span>{{ user2.email }}</span><br/>
			</div>
		</section>
	</main>
</template>

<script>
export default {
	name: 'home',
	data () {
		return {
			userData: [],
			userData2: []
		}
	},
	methods: {
		getRandomUsers () {
			let url = 'https://randomuser.me/api/?results=10';
			let result = fetch(url).then((requests) => {
				return requests.json()
			}).then(res => {
				return res.results
			})
			return result
		},
		getBlossomUsers (userIds) {
			let url = 'https://blossom-sample-api.herokuapp.com/users/'
			let requests = userIds.map(u => fetch(url + u)) // API returns array of promises
			return requests
		}
	},
	created () {
		console.info('created')

		this.userData = []
		this.userData2 = []

		this.getRandomUsers().then(res => {
			res.map(user => {
				this.userData.push({
					email: user.email,
					name: user.name.title + ' ' + user.name.first + ' ' + user.name.last
				})
			})
		}).catch(err => {
			console.error(err)
		})

		let userIds = [1, 2, 3]
		let promises = this.getBlossomUsers(userIds)
		Promise.all(promises).then(responses => {
			let res = responses.map(r => r.json());
			Promise.all(res).then(users => {
				users.map(user => {
					this.userData2.push({
						email: user.username,
						name: user.title + ' ' + user.name
					})
				})
			})
		}).catch(err => {
			console.error(err)
			console.log('promises', promises)
		})
	},
	mounted () {
		console.info('mounted')
	}
}
</script>

<style>

</style>
