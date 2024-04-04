<template>
  <div class="container">
    <div class="row">
      <h1>Case Study Data</h1>
      <div>
        <label>
          City Name:
          <select v-model="selectedCity">
            <option value="">Select City</option>
            <option v-for="city in cities" :key="city">{{ city }}</option>
          </select>
        </label>
        <label>
          Country Name:
          <select v-model="selectedCountry">
            <option value="">Select Country</option>
            <option v-for="country in countries" :key="country">
              {{ country }}
            </option>
          </select>
        </label>
        <button @click="applyFilter">Filter</button>
        <button @click="applyBackendFilter">Backend Filter</button>
        <button @click="resetFilter">Reset</button>
      </div>
    </div>
    <div class="row">
      <table>
        <thead>
          <tr>
            <th>Rank</th>
            <th>Hospital Name</th>
            <th>Country</th>
            <th>City</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="item in filteredCaseStudies" :key="item._id">
            <td>{{ item.rank }}</td>
            <td>{{ item.hospitalName }}</td>
            <td>{{ item.country }}</td>
            <td>{{ item.city }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      caseStudies: [],
      filteredCaseStudies: [],
      cities: [],
      countries: [],
      selectedCity: "",
      selectedCountry: "",
    };
  },
  mounted() {
    this.fetchData();
  },
  methods: {
    async fetchData() {
      try {
        const response = await fetch("http://localhost/case-study/");
        if (!response.ok) {
          throw new Error("Something went wrong! Couldn't fetch data.");
        }
        const data = await response.json();
        this.caseStudies = data;
        this.filteredCaseStudies = data;
        this.extractUniqueValues();
      } catch (error) {
        console.error("Error fetching data:", error);
      }
    },
    extractUniqueValues() {
      this.cities = Array.from(
        new Set(this.caseStudies.map((item) => item.city))
      );
      this.countries = Array.from(
        new Set(this.caseStudies.map((item) => item.country))
      );
    },
    applyFilter() {
      this.filteredCaseStudies = this.caseStudies.filter((item) => {
        return (
          (this.selectedCity === "" || item.city === this.selectedCity) &&
          (this.selectedCountry === "" || item.country === this.selectedCountry)
        );
      });
    },
    resetFilter() {
      this.selectedCity = "";
      this.selectedCountry = "";
      this.applyFilter();
    },
    async applyBackendFilter() {
      try {
        // Attention: The backend needs to be adjusted to accept query parameters
        const response = await fetch(
          `http://localhost/case-study/?city=${this.selectedCity}&country=${this.selectedCountry}`
        );
        if (!response.ok) {
          throw new Error("Something went wrong! Couldn't fetch data.");
        }
        const data = await response.json();
        this.filteredCaseStudies = data;
      } catch (error) {
        console.error("Error fetching data:", error);
      }
    },
  },
};
</script>

<style>
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
}
button {
  margin-left: 3px;
  margin-right: 3px;
}

.row {
  margin-bottom: 20px;
}

th,
td {
  padding: 8px;
  border-bottom: 1px solid #ddd;
  text-align: left;
}

th {
  background-color: #f2f2f2;
}
</style>
