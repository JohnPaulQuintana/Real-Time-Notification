<template>
	<!-- <div class="card-body" v-if="notifications">
		<div class="alert alert-info alert-dismissible fade show" id='redAlert' role="alert" v-for="notify in notifications" :key="notify">
			<strong>{{ notify.data.user_name }}</strong> Liked your post <b>{{ notify.data.post_title }}</b>
			<button type="button" class="form-control btn btn-primary" @click="read(notify.data.post_id)">mark as read</button>
		</div>
	</div> -->
	<div class="dropdown alert alert-danger alert-dismissible fade show" id='redAlert' role="alert" v-if="notifications">
		<!-- <a class="btn btn-secondary dropdown-toggle" type="button" id="dropdownMenuButton2" data-bs-toggle="dropdown" aria-expanded="false">
			{{ notifications.length }}<i class="fa-solid fa-bell"></i>
		</a> -->
		<span class="badge badge-danger text-danger" style="position:absolute; margin:-15px 0 0 -20px; font-size: 12pt;" v-show="notifications.length != 0"><b>{{ notifications.length }}</b></span><i class="fa-solid fa-bell dropdown-toggle" id="dropdownMenuButton2" data-bs-toggle="dropdown" aria-expanded="false"></i>
		<ul class="dropdown-menu dropdown-menu-light" aria-labelledby="dropdownMenuButton2" style="max-height:20em; overflow-y: auto;">
			<hr class="border border-success"/>
			<!-- {{ count }} -->
			<li v-for="(notify) in notifications" :key="notify" class="p-2">
				<strong class="text-success">{{ notify.data.user_name }}</strong> Liked your post <b class="text-success">{{ notify.data.post_title }}</b>
				<hr class="border border-success"/>
			</li>
				
		</ul>
	</div>
</template>

<script>
	import { ref, onMounted, reactive } from 'vue';

	export default{
		props:['user','user_notifications','notifications'],
	    setup(props){
	    	let users = ref([])
	    	let notifications = ref([])
			let count = ref(0)
	    	onMounted(() =>{
	    		notifications.value = props.user_notifications
				// count.value = notifications.value.length

				count.value = reactive(notifications.value.length)
	    		console.log(notifications.value);
	    	})

	    	Echo.private('post_like.'+props.user.id)
	          .notification((notification) => {
	            notifications.value.push(notification.notification);
	            console.log(notification);
	        })

			function read(id){
				console.log(id)
				axios.get('/post-like/read',{id})
				.then(response => {
	                console.log(response.data);
	            })
				.catch(err=>{
					console.log(err)
				})
			}

	        return {
	        	notifications, read, count
	        }
	    }
	}
</script>