<template>
  <ValidationObserver v-slot="{ invalid, validated }">
    <v-navigation-drawer v-model="showCreateEdit" app clipped right width="500">
      <template v-slot:prepend>
        <v-list-item two-line>
          <v-list-item-content>
            <v-list-item-title v-if="id" class="title">Edit</v-list-item-title>
            <v-list-item-title v-else class="title">New</v-list-item-title>
            <v-list-item-subtitle>Incident Type</v-list-item-subtitle>
          </v-list-item-content>
          <v-btn
            icon
            color="primary"
            :loading="loading"
            :disabled="invalid || !validated"
            @click="save()"
          >
            <v-icon>save</v-icon>
          </v-btn>
          <v-btn icon color="secondary" @click="closeCreateEdit()">
            <v-icon>close</v-icon>
          </v-btn>
        </v-list-item>
      </template>
      <v-card flat>
        <v-card-text>
          <v-container grid-list-md>
            <v-layout wrap>
              <v-flex xs12>
                <span class="subtitle-2">Details</span>
              </v-flex>
              <v-flex xs12>
                <ValidationProvider name="Name" rules="required" immediate>
                  <v-text-field
                    v-model="name"
                    slot-scope="{ errors, valid }"
                    :error-messages="errors"
                    :success="valid"
                    label="Name"
                    hint="A name for your incident type."
                    clearable
                    required
                  />
                </ValidationProvider>
              </v-flex>
              <v-flex xs12>
                <ValidationProvider name="Description" rules="required" immediate>
                  <v-textarea
                    v-model="description"
                    slot-scope="{ errors, valid }"
                    label="Description"
                    :error-messages="errors"
                    :success="valid"
                    hint="A description for your incident type."
                    clearable
                    required
                  />
                </ValidationProvider>
              </v-flex>
              <v-flex xs12>
                <v-select
                  v-model="visibility"
                  label="Visibility"
                  :items="visibilities"
                  hint="A visibility for your incident type"
                  clearable
                />
              </v-flex>
              <v-flex xs12>
                <service-select v-model="commander_service" />
              </v-flex>
              <v-flex xs12>
                <document-select v-model="template_document" />
              </v-flex>
            </v-layout>
          </v-container>
        </v-card-text>
      </v-card>
    </v-navigation-drawer>
  </ValidationObserver>
</template>

<script>
import { mapFields } from "vuex-map-fields"
import { mapActions } from "vuex"
import { ValidationObserver, ValidationProvider, extend } from "vee-validate"
import { required } from "vee-validate/dist/rules"
import ServiceSelect from "@/service/ServiceSelect.vue"
import DocumentSelect from "@/document/DocumentSelect.vue"

extend("required", {
  ...required,
  message: "This field is required"
})

export default {
  name: "IncidentTypeNewEditSheet",

  components: {
    ValidationObserver,
    ValidationProvider,
    ServiceSelect,
    DocumentSelect
  },

  data() {
    return {
      visibilities: ["Open", "Restricted"]
    }
  },

  computed: {
    ...mapFields("incident_type", [
      "dialogs.showCreateEdit",
      "selected.commander_service",
      "selected.description",
      "selected.id",
      "selected.loading",
      "selected.name",
      "selected.slug",
      "selected.template_document",
      "selected.visibility"
    ])
  },

  methods: {
    ...mapActions("incident_type", ["save", "closeCreateEdit"])
  }
}
</script>
