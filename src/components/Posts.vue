<template>
    <div class="input-group mb-3">
        <span class="input-group-text">&#128269;</span>
        <input 
            type="search" 
            class="form-control ds-input" 
            placeholder="Search posts..." 
            aria-label="Author"
            autocomplete="off"
            v-model="tempAuthor"
            @input="filterPosts"
        >
        <button 
            class="btn btn-outline-secondary fw-bold font-monospace" 
            type="button" 
            id="button-addon1"
            @click="clearSearch"
        >
            &#10006;
        </button>
    </div>

    <div v-if="loadingPosts">
        <p>Loading...</p>
    </div>
    <div v-else class="row">
        <div v-if="filteredPosts.length == 0">
            This author has no posts...
        </div>
        <div v-else v-for="post in filteredPosts" :key="post.id" class="col-12 col-md-6 col-lg-4 col-xl-3 mb-4">
            <div class="card bg-light text-dark">
                <div class="card-body text-bold">
                    <h4 class="card-title fw-bold font-monospace text-gray">{{ post.title }}</h4>
                    <p class="card-subtitle mb-2 text-black-50">{{ authors[post.userId - 1].name }}</p>
                    <p class="card-text">{{ post.body }}</p>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            loadingPosts: false,
            loadingAuthors: false,
            posts: [],
            filteredPosts: [],
            authors: [],
            searchAuthors: [],
            tempAuthor: '',
            matchAuthors: [],
        }
    },
    mounted() {
        this.loadingPosts = true
        this.loadingAuthors = true
        
        fetch('https://jsonplaceholder.typicode.com/posts')
            .then(response => response.json())
            .then(json => {
                this.posts = json
                this.filteredPosts = json
                this.loadingPosts = false
            })
            .catch(err => {
                console.log(err.message)
                this.loadingPosts = false
            })
        
        fetch('https://jsonplaceholder.typicode.com/users')
            .then(response => response.json())
            .then(json => {
                this.authors = json
                this.loadingAuthors = false
            })
            .catch(err => {
                console.log(err.message)
                this.loadingAuthors = false
            })
    },
    methods: {
        addAuthor(e) {
            console.log(this.tempAuthor)
            if (e.key === 'Enter' && this.tempAuthor) {
                if (!this.searchAuthors.includes(this.tempAuthor)) {
                    this.searchAuthors.push(this.tempAuthor)
                }
                this.tempAuthor = ''
            }
        },
        deleteAuthor(author) {
            this.searchAuthors = this.searchAuthors.filter((item) => {
                return author !== item
            })
        },
        clearSearch() {
            this.filteredPosts = this.posts
            this.tempAuthor = ''
        },
        filterPosts() {
            const authorIds = this.getAuthorId(this.tempAuthor)

            this.filteredPosts = this.posts.filter(post => {
                return authorIds.indexOf(post.userId) !== -1
            })
        },
        getAuthorId(name) {
            const authorIds = []
            this.authors.forEach(author => {
                if (author.name.toLowerCase().includes(name.toLowerCase())) {
                    authorIds.push(author.id)
                }
            })
            
            return authorIds
        }
    }
}
</script>

<style>
.badge {
    color: #f4f4f4;
    margin: 0 5px 5px;
    cursor: pointer;
}
.badge span {
    display: inline-block;
    color: #eee;
    font-size: 15px;
}
.hint {
    color: #555 !important;
    text-transform: lowercase;
    font-weight: bold !important;
    font-size: 15px !important;
}
</style>