<template>
  <div id="table-display">
    <h1>Dynamically-generated employees table</h1>
    <table>
      <thead>
        <th>Name</th>
        <th>Email</th>
      </thead>
      <tbody>
        <!--  Vue (like React) has a requirement for uniquely identifying any element 
        in an array, so we'll use :key -->
        <tr v-for="employee in employees" :key="employee.id">
          <td>{{ employee.name }}</td>
          <td>{{ employee.email }}</td>
        </tr>
      </tbody>
    </table>
    <h1>Hardcoded employees table</h1>
    <table>
      <thead>
        <tr>
          <th>Employee name</th>
          <th>Employee email</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td>Richard Hendricks</td>
          <td>richard@piedpiper.com</td>
        </tr>
        <tr>
          <td>Bertram Gilfoyle</td>
          <td>gilfoyle@piedpiper.com</td>
        </tr>
        <tr>
          <td>Dinesh Chugtai</td>
          <td>dinesh@piedpiper.com</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
  export default {
    // In Vue, the convention is that the filename and import will be in PascalCase,
    // i.e. "EmployeeTable", but in a template, this becomes kebab-case, "employee-table"
    name: "employee-table",
    props: {
      // Each prop is defined with a file type
      employees: Array,
    },
  };
</script>

<style scoped>
  #table-display {
    padding: 0 5rem 0 5rem;
  }
</style>
