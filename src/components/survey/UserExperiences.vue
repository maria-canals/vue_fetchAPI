<template>
  <section>
    <base-card>
      <h2>Submitted Experiences</h2>
      <div>
        <base-button @click="loadLearningExperiences"
          >Load Submitted Experiences</base-button
        >
      </div>
      <ul>
        <p v-if="isLoading">Loading...</p>
        <p v-if="!isLoading && error">There was an error... try again</p>
        <p v-if="!error && !isLoading && results.length === 0">
          No experiences added.
        </p>
        <survey-result
          v-else
          v-for="result in results"
          :key="result.id"
          :name="result.name"
          :rating="result.rating"
        ></survey-result>
      </ul>
    </base-card>
  </section>
</template>

<script>
import SurveyResult from './SurveyResult.vue';

export default {
  components: {
    SurveyResult,
  },
  data() {
    return {
      results: [],
      isLoading: false,
      error: null,
    };
  },
  mounted() {
    this.loadLearningExperiences();
  },
  methods: {
    loadLearningExperiences() {
      const URL = 'https://notas-f8fc4-default-rtdb.firebaseio.com/docs.json';

      const postOptions = {
        method: 'GET',
      };
      this.isLoading = true;
      this.error = null;
      fetch(URL, postOptions)
        .then((response) => response.json())
        .then((data) => {
          this.isLoading = false;
          const results = [];
          for (const id in data) {
            results.push({
              name: data[id].name,
              id: id,
              rating: data[id].rating,
            });
          }
          this.results = results;
        })
        .catch((error) => {
          this.isLoading = false;
          this.error = true;
          console.log('ha ocurrido un error' + error);
        });
    },
  },
};
</script>

<style scoped>
ul {
  list-style: none;
  margin: 0;
  padding: 0;
}
</style>