<script setup>
import PostList from "../components/PostList.vue";
import { ref, reactive, computed, watch, watchEffect } from "vue";

const posts = ref([
  { title: "跟米斯特吴学习全新的Vue.js", body: "vue", id: 1 },
  {
    title: "跟米斯特吴学习全新的React.js",
    body: "react Lorem ipsum dolor sit amet consectetur adipisicing elit. Explicabo, omnis reprehenderit. Explicabo incidunt maxime repellendus voluptatum earum ipsum veritatis excepturi, numquam omnis totam soluta quo cupiditate? Praesentium rerum quaerat quod obcaecati est, consectetur ut? Quis, libero similique temporibus reprehenderit sed dolor repellat vel amet rem nisi eius? Ratione dolores ipsa, temporibus autem voluptate aperiam in delectus laboriosam nostrum dolore vitae quo nihil corporis sunt atque soluta similique iure error labore porro dignissimos consequuntur qui ea. Recusandae expedita dolor odio ipsa illo culpa harum quas, rerum asperiores, vel, ullam est omnis quod sit. Vero ipsum natus placeat modi quaerat ullam earum eius! Explicabo, sunt! Aperiam totam alias, facere, expedita commodi maiores deserunt ipsa nihil debitis veniam, similique sequi soluta pariatur laudantium perferendis obcaecati nobis excepturi vitae molestias. Pariatur consequatur perspiciatis vitae ut nemo voluptates voluptas, consectetur adipisci distinctio id voluptate similique numquam eligendi, laborum modi quia facere quae accusantium velit suscipit. Ipsum debitis ea accusamus, ratione odit molestias eius, praesentium, reiciendis temporibus quasi eligendi neque iste quos voluptatum fugit magnam exercitationem ipsam cumque quo esse incidunt! Accusamus nam fugit vel rerum quisquam fugiat fuga quaerat dolor, quidem quae autem ipsum culpa dolore illum. Magnam dolores excepturi voluptates quasi perferendis voluptatibus iste!",
    id: 2,
  },
]);
</script>

<template>
  <div class="home">
    <PostList :posts="posts" />
  </div>
</template>

<style>
</style>
