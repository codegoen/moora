<template>
  <v-datatable :title="title" :allow-filter="false" :filters="datatable.filters" :data="datatable.data" :columns="datatable.columns">
    <template #attributes>
      <app-link :href="`/reqruitment/${this.reqruitment.id}/criteria`" class="btn-red">
        Tambah Kriteria
      </app-link>
    </template>

    <template #description="{ item: { description } }">
      <v-not-available :value="description" />
    </template>

    <template #action="{ item: { id } }">
      <div class="flex space-x-2">
        <app-link :href="`/reqruitment/${this.reqruitment.id}/criteria/${id}`" class="bg-blue-500 p-2 rounded-md focus:ring-2 focus:ring-blue-500 focus:outline-none focus:ring-offset-2">
          <icon name="EyeIcon" type="solid" class="w-3 h-3 text-white" />
        </app-link>
        <app-link :href="`/reqruitment/${this.reqruitment.id}/criteria/${id}/edit`" class="bg-yellow-400 p-2 rounded-md focus:ring-2 focus:ring-yellow-400 focus:outline-none focus:ring-offset-2">
          <icon name="PencilIcon" type="solid" class="w-3 h-3 text-white" />
        </app-link>
        <button @click.prevent="openDeleteModal(id)" type="button" class="bg-red-500 p-2 rounded-md focus:ring-2 focus:ring-red-500 focus:outline-none focus:ring-offset-2">
          <icon name="TrashIcon" type="solid" class="w-3 h-3 text-white" />
        </button>
      </div>
    </template>
  </v-datatable>

  <v-modal ref="modalDelete">
    <template #content>
      <div class="p-5 text-center">
        <div class="rounded-full bg-red-200 flex items-center justify-center w-24 h-24 flex-shrink-0 mx-auto">
          <icon name="TrashIcon" type="solid" class="w-16 h-16 text-red-500" />
        </div>
        <div class="text-3xl mt-5">Apakah anda yakin?</div>
        <div class="text-gray-600 mt-2 leading-7">Tindakan ini akan menghapus kriteria secara permanen dan tidak dapat dikembalikan.</div>
      </div>
      <div class="p-5 flex justify-center space-x-2 mt-4">
        <button type="button" class="btn-close" @click="$refs.modalDelete.closeModal()">Tutup</button>
        <loading-button :loading="deleteLoading" @click="destroy" class="btn-red" as="button" type="button">Hapus</loading-button>
      </div>
    </template>
  </v-modal>
</template>

<script>
export default {
  props: {
    title: String,
    datatable: Object,
    reqruitment: Object,
  },
  data() {
    return {
      deleteLoading: false,
      deleteCriteriaId: null,
    }
  },
  methods: {
    openDeleteModal(id) {
      this.deleteCriteriaId = id;
      this.$refs.modalDelete.openModal()
    },
    destroy() {
      this.$inertia.delete(`/reqruitment/${this.reqruitment.id}/criteria/${this.deleteCriteriaId}`, {
        onSuccess: () => {
          this.$toast.success('Berhasil menghapus penerimaan');
          this.$refs.modalDelete.closeModal();
        }
      })
    }
  }
}
</script>

<style>

</style>