<template>
    <div class="search-bar">
      <input
        v-model="keywords"
        type="text"
        id="search"
        placeholder="Search for the city or state in the U.S.A."
      >
      <i class="fa-solid fa-magnifying-glass"></i>
    </div>
</template>

<script>
    // use <script> instead of <script setup> to catch the updated search value
    export default {
        name: 'SearchBar',
        props: ['search'],
        emits: ['onSearchHandling', 'restartSearchHandling'], 
        data() {
            return {
                keywords: ''
            }
        },
        watch: {
            keywords() {
                this.$emit('onSearchHandling', this.keywords)

                if(this.keywords !== this.search) {
                    this.$emit('restartSearchHandling', false)
                }
            },
            search() {
                this.keywords = this.search
            },
        }
    }
</script>

<style scoped lang="scss">
    @import '../scss/variables';

    .search-bar {
        position: relative;
        margin-bottom: $md;

        input {
            box-sizing: border-box;
            width: 100%;
            font-size: 18px;
        }

        i {
            position: absolute;
            top: 24px;
            right: $lg;
            font-size: $lg;
        }
    }

    @media screen and (max-width: 768px) {
        .search-bar {
            input {
                padding-right: 50px;
            }
        }
    }
</style>