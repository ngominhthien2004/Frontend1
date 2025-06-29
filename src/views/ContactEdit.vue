<template>
    <div v-if="contactLocal" class="page">
        <h4>Hiệu chỉnh Liên hệ</h4>
        <ContactForm :contact="contactLocal" @submit:contact="updateContact" @delete:contact="deleteContact" />
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
        props: {
            id: { type: String, required: true },
        },
        data() {
            return {
                contact: null,
                contactLocal: null,
                message: "",
            };
        },
        methods: {
            async getContact(id) {
                try {
                    const contact = await ContactService.get(id);
                    this.contact = contact;
                    this.contactLocal = { ...contact };
                } catch (error) {
                    console.log(error);
                    this.$router.push({
                        name: "notfound",
                        params: {
                            pathMatch: this.$route.path.split("/").slice(1)
                        },
                        query: this.$route.query,
                        hash: this.$route.hash,
                    });
                }
            },
            async updateContact(data) {
                try {
                    await ContactService.update(this.contact._id, { ...data, _id: this.contact._id });
                    alert('Liên hệ được cập nhật thành công.');
                    this.$router.push({ name: "contactbook" });
                } catch (error) {
                    console.log(error);
                }
            },
            async deleteContact() {
                if (confirm("Bạn muốn xóa Liên hệ này?")) {
                    try {
                        await ContactService.delete(this.contact._id);
                        this.$router.push({ name: "contactbook" });
                    } catch (error) {
                        console.log(error);
                    }
                }
            },
        },
        created() {
            this.getContact(this.id);
            this.message = "";
        },
    };
</script>
