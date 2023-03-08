<script setup>
import q from "../data/data.json";
import { ref, watch } from "vue";
import Card from "../components/Card.vue";
import gsap from "gsap";

const quizzes = ref(q);
const search = ref("");
const showCards = ref(false);

watch(search, () => {
  quizzes.value = q.filter((quiz) =>
    quiz.name.toLowerCase().includes(search.value.toLowerCase())
  );
});

const beforeEnter = (el) => {
  // card-enter-from
  el.style.opacity = 0;
  el.style.transform = "translateY(-150px)";
};

const enter = (el) => {
  // card-enter-to
  gsap.to(el, {
    y: 0,
    opacity: 1,
    duration: 0.3,
    delay: el.dataset.index * 0.3,
  });
};

const afterEnter = (el) => {
  // card-enter-active
};
</script>

<template>
  <div>
    <header>
      <h1>Quizzes</h1>
      <input type="text" placeholder="Search..." v-model.trim="search" />
    </header>
    <div class="options-container">
      <TransitionGroup
        appear
        @before-enter="beforeEnter"
        @enter="enter"
        @after-enter="afterEnter"
      >
        <Card
          v-for="(quiz, index) in quizzes"
          :key="quiz.id"
          :quiz="quiz"
          :data-index="index"
        />
      </TransitionGroup>
    </div>
  </div>
</template>

<style scoped>
header {
  margin-bottom: 10px;
  margin-top: 30px;
  display: flex;
  align-items: center;
}

header h1 {
  font-weight: bold;
  margin-right: 30px;
}

header input {
  border: none;
  background-color: rgba(128, 128, 128, 0.1);
  padding: 10px;
  border-radius: 5px;
}

.options-container {
  display: flex;
  flex-wrap: wrap;
  margin-top: 40px;
}
</style>
