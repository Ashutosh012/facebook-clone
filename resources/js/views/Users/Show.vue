<template>
    <div class="flex flex-col items-center">
        <div class="relative mb-8">
            <div class="w-100 h-64 overflow-hidden z-10">
                <img src="https://as.ftcdn.net/r/v1/pics/7b11b8176a3611dbfb25406156a6ef50cd3a5009/home/discover_collections/optimized/image-2019-10-11-11-36-27-681.jpg" alt="User background" class="object-cover w-full">
            </div>

            <div class="absolute flex items-center bottom-0 left-0 -mb-8 ml-12 z-20">
                <div class="w-32 ">
                    <img src="https://images.pexels.com/photos/736716/pexels-photo-736716.jpeg?auto=compress&cs=tinysrgb&dpr=1&w=500" alt="" class="object-cover w-32 h-32 border-4 border-gray-200 rounded-full shadow-lg">
                </div>
                <p class="text-2xl text-gray-100 ml-4">{{ user.data.attributes.name }}</p>
            </div>
        </div>

        <p v-if="postLoading">Loading posts....</p>
        <Post v-else v-for="post in posts.data" :key="post.data.post_id" :post="post" />
        <p v-if="!postLoading && posts.data.length<1">No posts found. Get Started ....</p>
    </div>
</template>

<script>

    import Post from '../../components/Post';

    export default {
        name:"Show",

        components: {
            Post
        },

        data: () => {
            return {
                user:null,
                posts:null,
                userLoading:true,
                postLoading:true,
            }
        },

        mounted() {
            axios.get('/api/users/' + this.$route.params.userId)
                .then(res => {
                    this.user = res.data;
                })
                .catch(error=>{
                    console.log('Unable to fetch user from system');
                })
                .finally(() => {
                    this.userLoading = false;
                });

            axios.get('/api/users/' + this.$route.params.userId + '/posts')
                .then(res => {
                    this.posts = res.data;
                })
                .catch(error=>{
                    console.log('Unable to fetch post');
                })
                .finally(() => {
                    this.postLoading = false;
                });
        }
    }
</script>

<style scoped>


</style>