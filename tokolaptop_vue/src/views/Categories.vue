<template>
	<div>
		<v-container grid-list-md>
			<v-subheader>
				Semua Kategori
			</v-subheader>
			<v-layout row wrap>
				<!-- lakukan perulangan pada properti categories -->
				<v-flex v-for="category in categories" xs6 :key="category.id">
					<v-card :to="'/category/'+ category.slug">
						<v-img v-if="category.image" :src="getImage('/categories/'+category.image)" height="150px">
							<v-container fill-height fluid pa-2>
								<v-layout fill-height>
									<v-flex xs12 align-end flexbox>
										<span class="title white--text text-block" v-text="category.name"></span>
									</v-flex>
								</v-layout>
							</v-container>
						</v-img>
						<v-card-actions>
							<v-spacer></v-spacer>
							<v-btn icon>
								<v-icon>favorite</v-icon>
							</v-btn>
							<v-btn icon>
								<v-icon>bookmark</v-icon>
							</v-btn>
							<v-btn icon>
								<v-icon>share</v-icon>
							</v-btn>
						</v-card-actions>
					</v-card>
				</v-flex>
			</v-layout>
		</v-container>
		<template>
			<div clas="text-xs-center">
				<!-- kode untuk link paging halaman -->
				<v-pagination v-model="page" @input="go" :length="lengthPage" :total-visible="4"></v-pagination>
			</div>
		</template>
	</div>
</template>

<style scoped>
	.text-block {
		position: absolute;
		bottom: 5px;
		left: 5px;
		background-color: black;
		padding-left: 5px;
		padding-right: 5px;
		opacity: 0.7;
		width: 100%;
	}
</style>

<script>
	export default {
		data () {
			return {
				categories: [],
				page: 0,
				lengthPage: 0
			}
		},
		methods: {
			go(){
				let url = '/categories'
				if(this.page>0) url = '/categories?page='+this.page
					this.axios.get(url)
					.then((response) => {
						let response_data = response.data
						let categories = response_data.data
						this.lengthPage = response_data.meta.last_page
						this.categories = categories
					})
				.catch((error) => {
					console.log(error.responses)
				})
			},
			getImage (image){
				if(image!=null && image.length>0){
					return process.env.VUE_APP_BACKEND_URL+"/images"+ image
				}
				// default image jika tidak ditemukan,
				// letakkan image ini pada folder /public/img di project vue
				return "/img/unavailable.jpg"
			},
		},
		created(){
			this.go()
		}
	}
</script>