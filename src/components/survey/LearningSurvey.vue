<template>
  <section>
    <base-card>
      <h2>How was you learning experience?</h2>
      <form @submit.prevent="submitSurvey">
        <div class="form-control">
          <label for="name">Your Name</label>
          <input
            type="text"
            id="name"
            name="name"
            required
            v-model.trim="enteredName"
          />
        </div>
        <h3>My learning experience was ...</h3>
        <div class="form-control">
          <input
            type="radio"
            id="rating-poor"
            value="poor"
            name="rating"
            required
            v-model="chosenRating"
          />
          <label for="rating-poor">Poor</label>
        </div>
        <div class="form-control">
          <input
            type="radio"
            id="rating-average"
            value="average"
            name="rating"
            v-model="chosenRating"
          />
          <label for="rating-average">Average</label>
        </div>
        <div class="form-control">
          <input
            type="radio"
            id="rating-great"
            value="great"
            name="rating"
            v-model="chosenRating"
          />
          <label for="rating-great">Great</label>
        </div>
        <div>
          <base-button>Submit</base-button>
        </div>
      </form>
      <p v-if="error">{{ error }}</p>
      <p v-if="correct">Sent correctly</p>
    </base-card>
  </section>
</template>

<script>
export default {
  data() {
    return {
      enteredName: '',
      chosenRating: null,
      error: null,
      correct: null,
    };
  },
  methods: {
    submitSurvey() {
      this.error = null;
      this.correct = true;
      const URL = 'https://notas-f8fc4-default-rtdb.firebaseio.com/docs.json';

      const postOptions = {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({
          name: this.enteredName,
          rating: this.chosenRating,
        }),
      };

      fetch(URL, postOptions).catch((error) => {
        this.error = 'there was an error, try later';
        console.log('ha ocurrido un error' + error);
      });
      this.correct = true;
      this.enteredName = '';
      this.chosenRating = null;
    },
  },
};
</script>

<style scoped>
.form-control {
  margin: 0.5rem 0;
}

input[type='text'] {
  display: block;
  width: 20rem;
  margin-top: 0.5rem;
}
</style>