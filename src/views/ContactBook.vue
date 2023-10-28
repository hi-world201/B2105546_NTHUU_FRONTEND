<template>
  <div class="page row">
    <div class="col-md-10">
      <input-search v-model="searchText"></input-search>
    </div>
    <div class="mt-3 col-md-6">
      <h4>Danh bạ <i class="fas fa-address-book"></i></h4>

      <contact-list
        v-if="filteredContactsCound > 0"
        :contacts="filteredContacts"
        v-model:activeIndex="activeIndex"
      ></contact-list>
      <p v-else>Không có liên hệ nào</p>

      <div class="mt-3 row d-flex justify-content-around align-items-center">
        <button class="btn btn-sm btn-primary" @click="refreshList()">
          <i class="fas fa-redo"></i> Làm mới
        </button>
        <button class="btn btn-sm btn-success" @click="goToAddContact()">
          <i class="fas fa-plus"></i> Thêm mới
        </button>

        <button class="btn btn-sm btn-danger" @click="removeAllContacts">
          <i class="fas fa-trash"></i> Xóa tất cả
        </button>
      </div>
    </div>

    <div class="mt-3 col-md-6">
      <div v-if="activeContact">
        <h4>Chi tiết liên hệ <i class="fas fa-address-card"></i></h4>
        <contact-card :contact="activeContact"></contact-card>
        <router-link
          :to="{
            name: 'contact.edit',
            params: { id: activeContact._id },
          }"
        >
          <span class="mt-2 badge badge-warning">
            <i class="fas fa-edit"></i> Hiệu chỉnh</span
          >
        </router-link>
      </div>
    </div>
  </div>
</template>

<script>
import ContactCard from "@/components/ContactCard.vue";
import InputSearch from "@/components/InputSearch.vue";
import ContactList from "@/components/ContactList.vue";
import ContactService from "@/services/contact.service.js";

export default {
  components: {
    ContactCard,
    InputSearch,
    ContactList,
  },

  data() {
    return {
      contacts: [],
      activeIndex: -1,
      searchText: "",
    };
  },

  watch: {
    searchText() {
      this.activeIndex = -1;
    },
  },

  computed: {
    contactStrings() {
      return this.contacts.map((contact) => {
        const { name, email, address, phone } = contact;
        return [name, email, address, phone].join("");
      });
    },

    filteredContacts() {
      if (!this.searchText) return this.contacts;
      return this.contacts.filter((_contact, index) =>
        this.contactStrings[index].includes(this.searchText)
      );
    },

    activeContact() {
      if (this.activeIndex < 0) return null;
      return this.filteredContacts[this.activeIndex];
    },

    filteredContactsCound() {
      return this.filteredContacts.length;
    },
  },

  methods: {
    async retrieveContacts() {
      try {
        this.contacts = await ContactService.getAll();
      } catch (err) {
        console.log(error);
      }
    },

    refreshList() {
      this.retrieveContacts();
      this.activeIndex = -1;
      this.searchText = "";
    },

    async removeAllContacts() {
      if (confirm("Bạn có muốn xóa tất cả liên hệ?")) {
        try {
          await ContactService.deleteAll();
          this.refreshList();
        } catch (err) {
          console.log(err);
        }
      }
    },

    goToAddContact() {
      this.$router.push({ name: "contact.add" });
    },
  },

  mounted() {
    this.refreshList();
  },
};
</script>

<style scoped>
.page {
  text-align: left;
  max-width: 750px;
  margin: auto;
}
</style>
