<template>
  <div id="employee-form">
    <!-- @submit is short for "v-on:submit" -->
    <!-- .prevent == event.preventDefault(). Useful since not using the default GET/POST methods provided by forms -->
    <form @submit.prevent="handleSubmit">
      <label for="employee-name">Employee name</label>
      <!-- v-model is Vue syntactic sugar for updating an input value with an onchange event. -->
      <!-- employee.name is defined in data() below -->
      <!-- :class treats the class as JS instead of plain string -->
      <!-- Here, :class refers to the "submitting" state and  "invalidName()" computed(). 
      If both are true, "has-error" causes the input box border to turn red -->
      <!-- @focus = onfocus, @keypress = onkeypress -->
      <input
        id="employee-name"
        v-model="employee.name"
        type="text"
        :class="{ 'has-error': submitting && invalidName }"
        @focus="clearStatus"
        @keypress="clearStatus"
      />
      <label for="employee-email">Employee Email</label>
      <input
        id="employee-email"
        v-model="employee.email"
        type="text"
        :class="{ 'has-error': submitting && invalidName }"
        @focus="clearStatus"
        @keypress="clearStatus"
      />

      <!-- v-if is a conditional if vue https://vuejs.org/v2/guide/conditional.html -->
      <!-- Here, it is used to control the display of success/error message -->
      <!-- "error && submitting && success" refer to state in data() -->
      <p v-if="error && submitting" class="error-message">
        ❗Please fill out all required fields
      </p>
      <p v-if="success" class="success-message">
        ✅ Employee successfully added
      </p>

      <button type="submit">Add Employee</button>
    </form>
  </div>
</template>

<script>
  export default {
    name: "employee-form",
    // Acts as "state" for this component
    data() {
      return {
        // submitting, error, and false are possible states for the form after verification
        submitting: false,
        error: false,
        success: false,
        employee: {
          name: "",
          email: "",
        },
      };
    },
    methods: {
      handleSubmit() {
        console.log(
          `Request made to submit employee form. Details: ${JSON.stringify(
            // Prints the submitted employee details to console
            this.employee,
            null,
            2
          )}`
        );

        // Note the use of "this" to call data() and method() within the same component
        this.submitting = true;
        this.clearStatus();
        if (this.invalidName || this.invalidEmail) {
          this.error = true;
          return;
        }

        // Emits broadcast a name of an event and its associated data to its parent component
        // Here, "add:employee" is the event and "this.employee" is the data
        this.$emit("add:employee", this.employee);

        console.log(
          `Employee form submitted successfully. Setting this.success == true`
        );
        this.success = true;
      },
      clearStatus() {
        this.success = false;
        this.error = false;
      },
    },
    computed: {
      // methods to verify form inputs which trigger upon calling handleSubmit()
      invalidName() {
        return this.employee.name === "";
      },

      invalidEmail() {
        return this.employee.email === "";
      },
    },
  };
</script>

<style scoped>
  form {
    margin-bottom: 2rem;
  }

  #employee-form {
    padding: 0 5rem 0 5rem;
  }

  /* CSS for Error handling messages */
  [class*="-message"] {
    font-weight: 500;
  }

  .error-message {
    color: #d33c40;
  }

  .success-message {
    color: #32a95d;
  }
</style>
