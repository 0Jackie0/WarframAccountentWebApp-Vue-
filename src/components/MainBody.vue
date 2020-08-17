<template>
    <div class="mainBody">
        <ItemList />
    </div>
</template>

<script lang="ts">
    import Vue from 'vue';
    import ItemList from './itemList/ItemList.vue';

    export default Vue.extend({
        name: "MainBody",
        components: {
            ItemList
        }

    })
</script>

<style>
.tagStyle
{
    font-family: 'Lexend Deca', sans-serif;
}

.mainBody
{
    margin: 0 auto;
    width: 95%;
    margin-top: 20px;
    font-family: 'Raleway', sans-serif;
    font-size: 20px;
}
.dropdownStyle
{
    font-family: 'Orbitron', sans-serif;
    font-size: 20px;
}
.dropdownStyle#fixed {
    -webkit-appearance: none;
}
</style>