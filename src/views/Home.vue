<template>
  <div class="home">
    <h1>{{ message }}</h1>

    <div>
      <h2>Add Contact</h2>
      First Name:
      <input type="text" v-model="newContactFirstName" />
      <br />
      Middle Name:
      <input type="text" v-model="newContactMiddleName" />
      <br />
      Last Name:
      <input type="text" v-model="newContactLastName" />
      <br />
      Email:
      <input type="text" v-model="newContactEmail" />
      <br />
      Phone Number:
      <input type="text" v-model="newContactPhoneNumber" />
      <br />
      Bio:
      <input type="text" v-model="newContactBio" />
      <button v-on:click="createContact()">Create</button>
    </div>

    <h2>Contacts Index</h2>
    <div v-for="contact in contacts">
      <h1>First Name: {{ contact.first_name }}</h1>
      <h2>Middle Name: {{ contact.middle_name }}</h2>
      <h3>Last Name: {{ contact.last_name }}</h3>
      <h4>Email: {{ contact.email }}</h4>
      <h4>Phone Number: {{ contact.phone_number }}</h4>
      <h4>Bio: {{ contact.bio }}</h4>
      <div>
        {{ contact }}
        <button v-on:click="updateContact(contact)">Edit Contact Information</button>
        <div v-if="productUpdateContact">
          <h4>Edit Contact</h4>
          First Name:
          <input type="text" v-model="contact.first_name" />
          <br />
          Middle Name:
          <input type="text" v-model="contact.middle_name" />
          <br />
          Last Name:
          <input type="text" v-model="contact.last_name" />
          <br />
          Email:
          <input type="text" v-model="contact.email" />
          <br />
          Phone Number:
          <input type="text" v-model="contact.phone_number" />
          <br />
          Bio:
          <input type="text" v-model="contact.bio" />
        </div>
      </div>
    </div>
  </div>
</template>

<style></style>

<script>
import axios from "axios";
export default {
  data: function() {
    return {
      message: "Contacts FrontEnd",
      contacts: [],
      newContactFirstName: "",
      newContactMiddleName: "",
      newContactLastName: "",
      newContactEmail: "",
      newContactPhoneNumber: "",
      newContactBio: ""
    };
  },
  created: function() {
    axios.get("/api/contacts").then(response => {
      console.log(response.data);
      this.contacts = response.data;
    });
  },
  methods: {
    createContact: function() {
      var params = {
        first_name: this.newContactFirstName,
        middle_name: this.newContactMiddleName,
        last_name: this.newContactLastName,
        email: this.newContactEmail,
        phone_number: this.newContactPhoneNumber,
        bio: this.newContactBio
      };
      axios
        .post("/api/contacts", params)
        .then(response => {
          console.log("Contact added!", response.data);
          this.contacts.push(response.data);
        })
        .catch(error => {
          console.log(error.response.data.errors);
        });
    },
    updateContact: function(contact) {
      var params = {
        first_name: contact.first_name,
        middle_name: contact.middle_name,
        last_name: contact.last_name,
        email: contact.email,
        phone_number: contact.phone_number,
        bio: contact.bio
      };
      axios
        .patch(`/api/contacts/${contact.id}`, params)
        .then(response => {
          console.log("Contact updated!", response.data);
        })
        .catch(error => {
          console.log(error.response.data.errors);
        });
    }
  }
};
</script>
