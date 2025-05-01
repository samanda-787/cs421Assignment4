<template>
  <div class="home">
    <h1 class="title">University API</h1>
    <p class="subtitle">
      API: <span class="node">{{ respondingNode }}</span> <br>

       handled by: <span class="node">{{ apiNode }}</span>
    </p>

    <div class="buttons">
      <button @click="fetchStudents">Students</button>
      <button @click="fetchSubjects">Subjects</button>
    </div>

    <div v-if="loading" class="loading">Loading data...</div>

    <div v-if="students.length > 0" class="section">
      <h2 class="section-title">👩‍🎓 Students List ({{ students.length }})</h2>
      <ul class="card-list">
        <li v-for="(student, index) in students" :key="index" class="card">
          <strong>{{ student.name }}</strong><br />
          <span>{{ student.program }}</span>
        </li>
      </ul>
    </div>

    <div v-if="Object.keys(subjects).length > 0" class="section">
      <h2 class="section-title">📘 Courses List</h2>
      <div v-for="(subjectList, year) in subjects" :key="year" class="year-section">
        <h3 class="year-heading">{{ year }}</h3>
        <ul class="card-list">
          <li v-for="(subject, index) in subjectList" :key="index" class="card">
            {{ subject }}
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'HomeView',
  data() {
    return {
      students: [],
      subjects: [],
      respondingNode: 'students & subjects', // Frontend container ID
      apiNode: 'REBECA', // Backend node that handled API request
      loading: false
    };
  },
  methods: {
    async fetchStudents() {
      this.loading = true;
      this.subjects = [];
      try {
  const response = await fetch('http://ec2-16-170-217-124.eu-north-1.compute.amazonaws.com/students');
  if (!response.ok) throw new Error(`HTTP error! status: ${response.status}`);
  const data = await response.json();
  this.students = data.data || data;
}
 catch (error) {
        console.error('Fetch students error:', error);
        alert('Failed to load students. Check console for details.');
      } finally {
        this.loading = false;
      }
    },
    async fetchSubjects() {
      this.loading = true;
      this.students = [];
      try {
        const response = await fetch('http://ec2-16-170-217-124.eu-north-1.compute.amazonaws.com/subjects');
        const result = await response.json();
        this.subjects = result.data || {};
        this.apiNode = response.headers.get('X-Node-ID') || 'Unknown';
      } catch (error) {
        console.error('Subjects fetch error:', error);
        alert('Error loading subjects. See console for details.');
      } finally {
        this.loading = false;
      }
    }
  }
};
</script>

<style scoped>
.home {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;
  padding: 150px;
  box-sizing: border-box;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  color: #333;
  text-align: center;
}



.title {
  font-size: 2.4rem;
  margin-bottom: 10px;
  text-align: center;
}

.subtitle {
  text-align: center;
  color: #666;
  margin-bottom: 30px;
}

.node {
  font-weight: bold;
  color: #2c3e50;
}

.buttons {
  display: flex;
  justify-content: center;
  gap: 20px;
  margin-bottom: 30px;
}

button {
  padding: 12px 20px;
  background-color: #3498db;
  color: white;
  font-size: 1rem;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  transition: background-color 0.3s;
}


button:hover {
  background-color: #2980b9;
}

.section {
  margin-bottom: 40px;
}


.section, .year-section,
.card-list {
  width: 100%;
  max-width: 600px;
}

.section-title {
  font-size: 1.6rem;
  margin-bottom: 20px;
  color: #2c3e50;
}

.year-section {
  margin-bottom: 25px;
}

.year-heading {
  font-size: 1.3rem;
  color: #34495e;
  margin-bottom: 10px;
}

.card-list {
  list-style: none;
  padding: 0;
}

.card {
  background: #f9f9f9;
  border: 1px solid #e1e1e1;
  border-radius: 8px;
  padding: 15px 20px;
  margin-bottom: 10px;
  box-shadow: 0 2px 5px rgba(0,0,0,0.05);
  transition: background 0.2s ease;
  text-align: left;
}

.card:hover {
  background: #f1faff;
}

.loading {
  text-align: center;
  font-size: 1.1rem;
  color: #888;
}

.no-data {
  text-align: center;
  font-size: 1.1rem;
  color: #c0392b;
}
</style>