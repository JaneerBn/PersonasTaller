<template>
    <div class="container">
        <h1 class="text-start">Listado Municipios |
            <button @click="newMunicipio()" class="btn btn-success mx-2">
                <font-awesome-icon icon="plus" />
            </button>
        </h1>
        <table class="table">
            <thead>
                <tr>
                    <th scope="col">#</th>
                    <th scope="col">Code</th>
                    <th scope="col">Name</th>
                    <th scope="col">departamento</th>
                    <th scope="col">Actions</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(municipio, index) in municipios" :key="index">
                    <th scope="row">{{ index + 1 }}</th>
                    <td>{{ municipio.muni_codi }}</td>
                    <td>{{ municipio.muni_nomb }}</td>
                    <td>{{ municipio.depa_nomb }}</td>
                    <td>
                        <button @click="deleteMunicipio(municipio.muni_codi)" class="btn btn-danger mx-2">
                            <font-awesome-icon icon="trash" />
                        </button>
                        <button @click="editMunicipio(municipio.muni_codi)" class="btn btn-warning mx-2">
                            <font-awesome-icon icon="pencil" />
                        </button>
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
</template>
<script>
import axios from 'axios'
import Swal from 'sweetalert2'


export default {
    name: 'Municipio',
    data() {
        return {
            municipios: []
        }
    },
    methods: {
        deleteMunicipio(codigo) {
            Swal.fire({
                title: 'Do you want to delete the Municipio with id ${codigo}?',
                showCancelButton: true,
                confirmButtonText: 'Delete',
            }).then((result) => {
                /* Read more about isConfirmed, isDenied below */
                if (result.isConfirmed) {
                    // Swal.fire('Saved!', '', 'success')
                    axios.delete(`http://127.0.0.1:8000/api/municipios/${codigo}`)
                        .then(response => {
                            if (response.data.success) {
                                Swal.fire('Deleted!!', '', 'success')
                                this.municipios = response.data.municipios
                            }
                        })
                }
            })
        },
        editMunicipio(id) {
            this.$router.push({ name: 'EditarMunicipio', params: { id: `${id}` } })
        },
        newMunicipio() {
            this.$router.push({ name: 'NewMunicipio' });
        }
    },
    mounted() {
        axios
            .get('http://127.0.0.1:8000/api/municipios')
            .then(response => (this.municipios = response.data.municipios))
    },
}
</script>