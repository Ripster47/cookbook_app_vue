<template>
  <div class="container">
    <h1>New Contact</h1>
    <div>
      <div>First Name: <input type="text" v-model="newFirstName"></div>
      <div>Last Name: <input type="text" v-model="newLastName"></div>
      <div>Email: <input type="text" v-model="newEmail"></div>
      <div>Phone Number: <input type="text" v-model="newPhoneNumber"></div>
      <div>Bio: <input type="text" v-model="newBio"></div>
      <button v-on:click="createContact()">Create New Contact</button>
    </div>
    <h1>All Contacts</h1>
    <div v-for="contact in contacts">
      <h2>Full Name: {{ contact.first_name }} {{ contact.last_name }}</h2>
      <button v-on:click="showContact(contact)">Show more</button>
           <div v-if="currentContact === contact">
             <p>First Name: {{ contact.first_name }} </p>
             <p>Last Name: {{ contact.last_name }} </p>
             <p>Email: {{ contact.email }} </p>
             <p>Phone Number: {{ contact.phone_number }} </p>
             <p>Bio: {{ contact.bio }} </p>
           </div>
      <!-- <p>Email: {{ contact.email }}</p>
      <p>Phone Number: {{ contact.phone_number }}</p>
      <p>Bio: {{ contact.bio }}</p> -->
    </div>
  </div>
</template>

<script>
  import axios from "axios";

  export default {
    data: function() {
      return {
        contacts: [],
        newFirstName: "",
        newLastName: "",
        newEmail: "",
        newPhoneNumber: "",
        newBio: "",
        currentContact: {}
      };
    },
    created: function() {
      axios.get("/api/contacts").then(response => {
        this.contacts = response.data;
      });
    },
  methods: {
    createContact: function() {
      var params = {
        first_name: this.newFirstName,
        last_name: this.newLastName,
        email: this.newEmail,
        phone_number: this.newPhoneNumber,
        bio: this.newBio
      };
      axios.post("/api/contacts", params)
      .then(response => {
        this.contacts.push(response.data);
        this.newFirstName;
        this.newLastName;
        this.newEmail;
        this.newPhoneNumber;
        this.newBio;
        });
      },
      showContact: function(inputContact) {
        if (this.currentContact === inputContact) {
          this.currentContact = {};
        } else {
          this.currentContact = inputContact;
        }
      }
    }
  };

</script>
