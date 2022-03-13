<template>
  <section>
    <base-card>
      <h2>Submitted Experiences</h2>

      <div>
        <base-button @click="loadExperiences"
          >Load Submitted Experiences</base-button
        >
      </div>

      <ul>
        <survey-result
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

const DATABASE = process.env.VUE_APP_DATABASE;

export default {
  // props: ['results'],

  data() {
    return {
      results: [],
    };
  },

  components: {
    SurveyResult,
  },

  methods: {
    loadExperiences() {
      fetch(DATABASE + 'surveys.json')
        .then((response) => {
          if (response.ok) {
            return response.json();
          }
        })
        .then((data) => {
          // Transform the data into a format that is easier to work with.
          this.results = Object.keys(data).map((key) => {
            return {
              id: key,
              name: data[key].userName,
              rating: data[key].rating,
            };
          });
        });
    },
  },

  mounted() {
    this.loadExperiences();
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
