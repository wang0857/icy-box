<script setup>
    defineProps({
        search: String,
        searchedCities: Array,
        hasSetResult: Boolean
    })

    const emit = defineEmits(['onResultHandling'])

    function resultHandler(e) {
        emit('onResultHandling', e.target.innerText)
    }
</script>

<template>
    <p
        v-if="searchedCities.length === 0 && search !== ''"
    >
        No result. Try another one.
    </p>
    <div
        v-if="searchedCities.length !== 0 && search !== '' && !hasSetResult"
        class="results-container"
    >
      <ul class="results-list">
        <li
          v-for="item, index in searchedCities"
          :key="item.rank"
          :tabindex="index + 1"
          class="results-list-item"
          @click="resultHandler"
          @keyup.enter="resultHandler"
        >
          {{ item.city }}, {{ item.state }}
        </li>
      </ul>
    </div>
</template>

<style scoped lang="scss">
    @import '../scss/variables';

    .results {
        &-container {
            width: 100%;
            max-height: 380px;
            padding: $lg;
            border-radius: $xlg;
            background-color: $white;
            box-sizing: border-box;
            box-shadow: $box-shadow;
            overflow-y: scroll;
        }

        &-list {
            margin: 0px;
            padding: 0px;
            list-style: none;

        &-item {
            padding: 3px $md;
            border-radius: $md;

            &:hover {
            background-color: $very-light-gray-cyan;
            cursor: pointer;
            }

            &:focus,
            &:focus-visible {
                outline: 2px solid $cyan;
                outline-offset: 3px;
            }
        }
        }

    }
</style>