<template>
  <div class="contact-form">
    <h4 class="headline-4 mb-5" id="contact">Contact</h4>
    <form name="contact_form" class="form text-left" @submit="sendForm" method="post">
    <div class="row">
      <div class="col">
        <label for="first_name">First Name</label>
        <input type="text" id="first_name" class="form-control" value="" required>
      </div>
      <div class="col">
        <label for="last_name">Last Name</label>
        <input type="text" id="last_name" class="form-control" value="" required>
      </div>
    </div>
    <div class="form-group">
      <label for="email_field">Email address</label>
      <input type="email" class="form-control" id="email_field" value="" required>
    </div>
    <div class="form-group">
      <label for="message_field">Message</label>
      <textarea class="form-control" id="message_field" rows="3" value="Hello!" required></textarea>
    </div>
      <div class="row justify-content-start">
        <div class="col-9">
          <div class="d-inline-flex">
            <i v-if="sent === false" class="iconify fail" data-icon="mdi:exclamation-thick" />
            <p v-if="sent === false" class="notsent">Oops. Something went wrong. Please contact us directly or try again.</p>
          </div>
          <div class="d-inline-flex align-content-start">
            <i v-if="sent === true" class="iconify success" data-icon="mdi:check" />
            <p v-if="sent === true" class="sent">Message sent!</p>
          </div>
        </div>
        <div class="col-2 align-content-end">
          <v-progress-circular class="spinner" v-show="indeterminate" :indeterminate="indeterminate" color="primary" />
        </div>
        <div class="col-1 d-flex justify-content-end">
          <button type="submit" class="btn btn-primary">Send</button>
        </div>
      </div>
    </form>
  </div>
</template>
<script>

export default {
  name: "Contact",
  data () {
    return {
      indeterminate: false,
      sent: null
    }
  },
  methods: {
    change_state(success) {
      this.sent = success
      this.indeterminate = false
    },
    sendForm:function(e) {
      this.sent = undefined
      this.indeterminate = true
      e.preventDefault();
      const form_data = document.forms["contact_form"];
      const first_name = form_data['first_name'].value;
      const last_name = form_data['last_name'].value;
      const user_mail = form_data['email_field'].value;
      const message = form_data['message_field'].value;
      const data_json = {subject: "NEW: City Data Visualizer", first_name, last_name, user_mail, message};

      const XHR = new XMLHttpRequest();
      const URL = 'https://cdv-mail.api.datexis.com/mail'

      // send data
      XHR.open('POST', URL);

      let loading = () => {
        if (XHR.status >= 200 && XHR.status < 300) {
          this.change_state(true)
        } else {
          console.warn(XHR.statusText, XHR.responseText);
          this.change_state(false)
        }
      }
      XHR.addEventListener( 'load', loading);
      XHR.setRequestHeader('Content-Type', 'application/json');

      XHR.send(JSON.stringify(data_json));
    }
  },
};
</script>

<style scoped>

.headline-4 {
  font-weight: 300;
  font-size: 3.5rem;
  line-height: 1.2;
}

.contact-form{
  background: #ffffff;
  display: inline-block;
  width: 50%;
  padding: 2%;
  border: 1px lightgray solid;
  border-radius: 1%;
}

.form .row {
  padding: 0px;
}
.btn-primary {
  color: white;
}
.spinner {
  width: 1.5rem;
  height: 1.5rem
}
.success {
  width: 1.5rem;
  height: 1.5rem;
  color: darkseagreen;
  margin-left: 2rem;
}

.fail {
  width: 1.5rem;
  height: 1.5rem;
  color: red;
  margin-left: 2rem;
}
sent {
  color: darkseagreen;
}

notsent {
  color: red;
}

p {
  margin-left: 0.5rem;
}
.d-inline-flex {
  margin-top: 0.6rem;
}
</style>
