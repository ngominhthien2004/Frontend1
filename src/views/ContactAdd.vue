<template>
    <div class="page">
        <h4>Thêm liên hệ mới</h4>
        <ContactForm
            :contact="contact"
            @submit:contact="createContact"
            @delete:contact="goBack"
        />
    </div>
</template>

<script>
import ContactForm from "@/components/ContactForm.vue";
import contactService from "@/services/contact.service";

export default {
    components: { ContactForm },
    data() {
        return {
            contact: {
                name: "",
                email: "",
                address: "",
                phone: "",
                favorite: false,
            },
        };
    },
    methods: {
        async createContact(contact) {
            await contactService.create(contact);
            this.$router.push({ name: "contactbook" });
        },
        goBack() {
            this.$router.push({ name: "contactbook" });
        },
    },
};
</script>
