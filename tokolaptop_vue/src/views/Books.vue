<template>
	<div>
		<v-container grid-list-md>
			<v-subheader>
				Semua Produk Laptop
			</v-subheader>
			<v-layout row wrap>
				<v-flex v-for="(book, index) in books" xs6 :key="index">
					<v-card :to="'/book/'+ book.slug">
						<v-img :src="getImage('/books/'+book.cover)" height="150px">
							<v-container fill-height fluid pa-2>
								<v-layout fill-height>
									<v-flex xs12 align-end flexbox>
										<span class="title white--text text-block" v-text="book.title"></span>
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
				books: [],
				page: 0,
				lengthPage: 0
			}
		},
		methods: {
			go(){
				let url = '/books'
				if(this.page>0) url = '/books?page='+this.page
					this.axios.get(url)
					.then((response) => {
						let response_data = response.data
						let books = response_data.data
						this.lengthPage = response_data.meta.last_page
						this.books = books
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