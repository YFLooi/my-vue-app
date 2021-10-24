<template>
  <div>
    <h1>Employees</h1>
    <!-- @add: Acknowledge and capture the event "employee" being broadcasted by EmployeeForm -->
    <employee-form @add:employee="addEmployee" />
    <employee-table :employees="employees" />
  </div>
  <br />
  <br />
  <img alt="Vue logo" src="./assets/logo.png" />
  <!-- ":msg2" is short for "v-bind:msg2", a data-binding colon -->
  <!-- It binds the msg prop to the HelloWorld component's data -->
  <!-- Can also run certain JavaScript inside properties and attributes with the :, such as ternary statements and functions. -->
  <hello-world msg="Welcome to Your Vue.js App" :msg2="msg2" />
  <!-- This binds to msg2 in the data obj. Changing this input changes msg2 -->
  <input v-model="msg2" type="text" />
  <!-- @click = html onclick = shorthand for v-on:click="alertMessage" -->
  <button @click="alertMessage">Popup msg2</button>
</template>

<script>
  import HelloWorld from "@/components/HelloWorld.vue";
  import EmployeeTable from "@/components/EmployeeTable.vue";
  import EmployeeForm from "@/components/EmployeeForm.vue";

  export default {
    name: "App",
    components: {
      HelloWorld,
      EmployeeTable,
      EmployeeForm,
    },
    // When our Vue app runs, it adds all of the properties found in
    // this data object to Vue’s reactivity system
    // Hence, if the message returned by data() changes, anything downstream also changes
    // It thus behaves something like React's state
    data() {
      return {
        msg2: "Hello from data obj{}",
        employees: [
          {
            id: 1,
            name: "Yih Foo",
            email: "yihfoo@piedpiper.com",
          },
          {
            id: 2,
            name: "Chin Foo",
            email: "chinfoo@piedpiper.com",
          },
          {
            id: 3,
            name: "Fu Fu",
            email: "fufu@piedpiper.com",
          },
        ],
      };
    },
    methods: {
      alertMessage() {
        // "this" is used when accessing msg2 from the same component's methods
        // In JavaScript, "this" refers to the object that the function, method, or class belongs to.
        //  The only instances in which this. does not refer to the owner, global, or referred
        // object is when it is called in a function in strict mode (where it is undefined),
        // or within an event, in which case this. refers to the element that receives said event
        alert(this.msg2);
      },
      addEmployee(employee) {
        // Generate a unique id for the emloyee
        const lastId =
          this.employees.length > 0
            ? this.employees[this.employees.length - 1].id
            : 0;
        const id = lastId + 1;
        const newEmployee = { ...employee, id };

        this.employees = [...this.employees, newEmployee];
      },
      async getEmployees() {
        // fetch req in Vue. Can be axios too...
        try {
          const response = await fetch(
            "https://jsonplaceholder.typicode.com/users"
          );

          // Alternative fetch() syntax, here for a POST req:
          // const response = await fetch(
          //   "https://jsonplaceholder.typicode.com/users",
          //   {
          //     method: "POST",
          //     body: JSON.stringify(employee),
          //     headers: { "Content-type": "application/json; charset=UTF-8" },
          //   }
          // );
          // const data = await response.json();

          const data = await response.json();
          console.log(
            `Data on employees retrieved on component mount: ${JSON.stringify(
              data,
              null,
              2
            )}`
          );
          // this.employees = data;
        } catch (error) {
          console.error(error);
        }
      },
    },
    // Similar to compnentDidMount() in React: It acts after componet is inserted into DOM
    mounted() {
      this.getEmployees();
    },
  };
</script>

<style>
  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }

  button {
    background: #009435;
    border: 1px solid #009435;
  }

  .small-container {
    max-width: 680px;
  }
</style>
