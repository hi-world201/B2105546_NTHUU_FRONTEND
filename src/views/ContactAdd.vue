<template>
  <div class="page">
    <h4>Thêm Liên hệ</h4>
    <ContactForm @submit:contact="createContact" :contact="contact" />
    <p>{{ message }}</p>
  </div>
</template>

<script>
import ContactForm from "@/components/ContactForm.vue";
import ContactService from "@/services/contact.service";
export default {
  components: {
    ContactForm,
  },
  data() {
    return {
      contact: null,
      message: "",
    };
  },
  methods: {
    async createContact(data) {
      try {
        await ContactService.create(data);
        this.message = "Liên hệ được thêm thành công.";
        alert(this.message);
        this.$router.push({ name: "contactbook" });
      } catch (error) {
        console.log(error);
      }
    },
    refreshContact() {
      this.contact = {
        name: "",
        email: "",
        address: "",
        phone: "",
        favorite: false,
      };
    },
  },
  created() {
    this.refreshContact();
    this.message = "";
  },
};
</script>
