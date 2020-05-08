<template>
  <div>
    <!--Add transaction users--->
    <!--Transaction Users--->
    <md-dialog
      :md-active.sync="openAddTransactionModal"
      class="modal-medium"
      :md-click-outside-to-close="false"
    >
      <md-dialog-title class="modal-title d-block py-3 px-4 bg-light position-relative">
        <h4 class="my-0 md-headline text-left text-primary">Add transaction</h4>
        <md-button
          @click="openAddTransactionModal = false"
          class="md-icon-button position-absolute"
        >
          <md-icon class="text-primary">close</md-icon>
        </md-button>
      </md-dialog-title>
      <div class="md-dialog-content p-0 pb-2">
        <form @submit.prevent="addTransaction">
          <md-content md-dynamic-height class="px-4">
            <div class="md-layout">
              <!-- <div class="md-layout-item md-small-size-100 md-size-50">
                    <md-field slot="inputs" :class="app.getValidationClass($v.addForm,'first_name')">
                        <label>First name</label>
                        <md-input v-model="addForm.first_name" class="px-0"></md-input>
                        <span v-if="!$v.addForm.first_name.required" class="md-error">First name is required</span>
                    </md-field>
                </div>
                <div class="md-layout-item md-small-size-100 md-size-50">
                    <md-field slot="inputs" :class="app.getValidationClass($v.addForm,'last_name')">
                        <label>Last name</label>
                        <md-input v-model="addForm.last_name" class="px-0"></md-input>
                        <span v-if="!$v.addForm.last_name.required" class="md-error">Last name is required</span>
                    </md-field>
                </div>
                <div class="md-layout-item md-small-size-100 md-size-100">
                    <md-field slot="inputs" :class="app.getValidationClass($v.addForm,'description')">
                        <label>Description</label>
                        <md-textarea v-model="addForm.description" class="px-0"></md-textarea>
                        <span v-if="!$v.addForm.description.required" class="md-error">Description is required</span>
                        <span v-if="!$v.addForm.maxLength" class="md-error">maxLength is 300</span>
                    </md-field>
                </div>
                <div class="md-layout-item md-small-size-100 md-size-50">
                    <md-field slot="inputs" :class="app.getValidationClass($v.addForm,'url')">
                        <label>URL (Vimeo, Youtube, ...)</label>
                        <md-input v-model="addForm.url"></md-input>
                        <span v-if="!$v.addForm.url.urlValid" class="md-error">Url invalid</span>
                    </md-field>
              </div>-->
              <div class="md-layout-item md-small-size-100 md-size-50">
                <md-field
                  slot="inputs"
                  :class="app.getValidationClass($v.addForm,'transactional_id')"
                >
                  <label>Transactional ID</label>
                  <md-input v-model="addForm.transactional_id" :disabled="true"></md-input>
                  <!-- <span v-if="!$v.addForm.transactional_id.required" class="md-error">Transactional is required</span> -->
                </md-field>
              </div>
              <div class="md-layout-item md-small-size-100 md-size-50">
                <md-field
                  slot="inputs"
                  :class="app.getValidationClass($v.addForm,'transaction_name')"
                >
                  <label>Transaction Name</label>
                  <md-input v-model="addForm.transaction_name"></md-input>
                  <span
                    v-if="!$v.addForm.transaction_name.required"
                    class="md-error"
                  >Transaction name is required</span>
                </md-field>
              </div>
              <div class="md-layout-item md-small-size-100 md-size-50">
                <md-field
                  slot="inputs"
                  :class="app.getValidationClass($v.addForm,'transaction_type')"
                >
                  <label>Select Transaction Type</label>
                  <md-select id="role" v-model="addForm.transaction_type">
                    <md-option
                      :value="item"
                      v-for="(item,index) in transactionsTypesList"
                      :key="item.id"
                    >{{ index|capitalize }}</md-option>
                  </md-select>
                  <span
                    v-if="!$v.addForm.transaction_type.required"
                    class="md-error"
                  >Transaction type is required</span>
                </md-field>
              </div>
              <!-- <div class="md-layout-item md-small-size-100 md-size-50">
                            <md-field slot="inputs" :class="app.getValidationClass($v.addForm,'source')">
                                <label>Source</label>
                                <md-select id="role" v-model="addForm.source">
                                    <md-option value="1">Facebook</md-option>
                                    <md-option value="2">Instagram</md-option>
                                    <md-option value="3">other</md-option>
                                </md-select>
                            </md-field>
              </div>-->
              <div class="md-layout-item md-small-size-100 md-size-50 template-tag">
                <!-- <img src="../img/search-icon.svg" alt class="position-absolute search-icon" /> -->
                <!-- <img
                  :src="'../img/search-icon.svg'"
                  alt
                  style="width: 20px;"
                  class="position-absolute"
                />-->
                <div
                  slot="inputs"
                  :class="app.getValidationClass($v,'selectedTransactionTags')"
                  class="tag-with-multi-input search-tag-class md-field md-theme-default"
                >
                  <tags-input
                    element-id="tags"
                    :placeholder="'Transaction Template'"
                    v-model="selectedTransactionTags"
                    :existing-tags="existingTags"
                    :typeahead="true"
                    :typeahead-style="typeaheadStyle"
                    :allow-duplicates="allowDuplicates"
                    :typeaheadHideDiscard="typeaheadHideDiscard"
                    @tag-added="onTagAdded"
                    @change="onTagChange"
                  ></tags-input>
                  <!-- {{ $v.selectedTransactionTags }} -->
                  <div v-if="tagSearchResult" class="tag-not-found">
                    <ul class="typeahead-dropdown">
                      <li class="tags-input-typeahead-item-highlighted-default">Tag not found</li>
                    </ul>
                  </div>
                </div>
              </div>
            </div>
          </md-content>
          <md-dialog-actions class="flex-wrap justify-content-center">
            <!-- {{$v.addForm.$invalid}} -->
            <md-button type="submit" class="md-primary w-100 button-primary-custom mb-3 mt-3">
              <LoadingButtonLoader
                :typeEnable="'simpleLoader'"
                v-bind:enable="app.loading"
                class="mr-1"
              />Next
            </md-button>
            <!-- <div class="d-block w-100 text-center">
                        <md-button :md-ripple="false" @click="transactionSteps = false" class="cancel-link lighter-description mt-2 text-capitalize">skip</md-button>
            </div>-->
          </md-dialog-actions>
        </form>
      </div>
    </md-dialog>
    <md-dialog :md-active.sync="openTransactionDetails" :md-click-outside-to-close="false">
      <md-dialog-title class="modal-title d-block py-3 px-4 bg-light position-relative">
        <h4 class="my-0 md-headline text-left text-primary">Add Transaction Details</h4>
        <md-button @click="openTransactionDetails = false" class="md-icon-button position-absolute">
          <md-icon class="text-primary">close</md-icon>
        </md-button>
      </md-dialog-title>
      <div class="md-dialog-content pb-0" id="transaction-detail">
        <form @submit.prevent="addTransactionalDetails">
          <md-content md-dynamic-height class="px-0">
            <div class="md-layout">
              <div class="md-layout-item md-medium-size-100 md-size-100 md-small-size-100">
                <div class="md-layout">
                  <div class="md-layout-item md-small-size-100 md-size-50">
                    <md-field
                      slot="inputs"
                      :class="app.getValidationClass($v.transactionalForm,'sales_price')"
                      class="multi-error"
                    >
                      <label>Sales price</label>
                      <span class="md-prefix">$</span>
                      <md-input v-model="transactionalForm.sales_price"></md-input>
                      <span
                        v-if="!$v.transactionalForm.sales_price.required"
                        class="md-error"
                      >Sales price is required</span>
                      <span
                        v-if="!$v.transactionalForm.sales_price.priceValid.isfloat"
                        class="md-error"
                      >Please enter decimal or number</span>
                    </md-field>
                  </div>
                  <div class="md-layout-item md-small-size-100 md-size-50">
                    <md-field slot="inputs">
                      <label>Seller Assist</label>
                      <md-input v-model="transactionalForm.seller_assist"></md-input>
                      <!-- <span
                                        v-if="!$v.userForm.first_name.required"
                                        class="md-error"
                      >Firstname is required</span>-->
                    </md-field>
                  </div>
                  <div class="md-layout-item md-small-size-100 md-size-50">
                    <md-field
                      slot="inputs"
                      :class="app.getValidationClass($v.transactionalForm,'street_no')"
                    >
                      <label>Street No</label>
                      <md-input v-model="transactionalForm.street_no"></md-input>
                      <span
                        v-if="!$v.transactionalForm.street_no.required"
                        class="md-error"
                      >Street no is required</span>
                    </md-field>
                  </div>
                  <div class="md-layout-item md-small-size-100 md-size-50">
                    <md-field
                      slot="inputs"
                      :class="app.getValidationClass($v.transactionalForm,'street_address')"
                    >
                      <label>Street Address</label>
                      <md-input v-model="transactionalForm.street_address"></md-input>
                      <span
                        v-if="!$v.transactionalForm.street_address.required"
                        class="md-error"
                      >Street address is required</span>
                    </md-field>
                  </div>
                  <div class="md-layout-item md-small-size-100 md-size-50">
                    <md-field
                      slot="inputs"
                      :class="app.getValidationClass($v.transactionalForm,'state')"
                    >
                      <label>Select State</label>
                      <md-select
                        id="role"
                        v-model="transactionalForm.state"
                        @md-selected="stateSelect"
                      >
                        <md-option
                          :value="item.id"
                          v-for="(item) in stateLists"
                          :key="item.id"
                        >{{ item.state_name }}</md-option>
                      </md-select>
                      <span
                        v-if="!$v.transactionalForm.state.required"
                        class="md-error"
                      >State is required</span>
                    </md-field>
                  </div>
                  <div class="md-layout-item md-small-size-100 md-size-50">
                    <md-field
                      slot="inputs"
                      :class="app.getValidationClass($v.transactionalForm,'city')"
                      placeholder="Select city"
                      class="multi-error"
                    >
                      <label>Select city</label>
                      <LoadingButtonLoader
                        :typeEnable="'pageLoader'"
                        v-bind:enable="app.loading2"
                        class="mr-1 loader-select"
                      />
                      <md-select
                        id="addForm-city"
                        v-model="transactionalForm.city"
                        @md-selected="citySelect"
                      >
                        <md-option :value="0" v-if="citiesLists.length == 0">Choose city</md-option>
                        <md-option
                          :value="JSON.stringify({ id: item.id, id_state: item.id_state })"
                          v-for="(item) in citiesLists"
                          :key="item.id"
                        >{{ item.city }}</md-option>
                      </md-select>
                      <span
                        v-if="!$v.transactionalForm.city.required"
                        class="md-error"
                      >City is required</span>
                      <span
                        v-if="!$v.transactionalForm.city.isStateSelect"
                        class="md-error"
                      >Please select state first</span>
                    </md-field>
                  </div>
                  <div class="md-layout-item md-small-size-100 md-size-50">
                    <md-field slot="inputs">
                      <label>Zip code</label>
                      <md-input v-model="transactionalForm.zip_code"></md-input>
                      <!-- <span
                                            v-if="!$v.userForm.last_name.required"
                                            class="md-error"
                      >Lastname is required</span>-->
                    </md-field>
                  </div>
                  <div class="md-layout-item md-small-size-100 md-size-50">
                    <md-field slot="inputs">
                      <label>MLS No.</label>
                      <md-input v-model="transactionalForm.mls_no"></md-input>
                      <!-- <span
                                            v-if="!$v.userForm.last_name.required"
                                            class="md-error"
                      >Lastname is required</span>-->
                    </md-field>
                  </div>

                  <div class="md-layout-item md-small-size-100 md-size-50">
                    <md-field
                      slot="inputs"
                      class="multi-error datepicker-md-field"
                      :class="app.getValidationClass($v.transactionalForm,'executed_date')"
                    >
                      <md-datepicker
                        v-model="transactionalForm.executed_date"
                        @md-closed="changeDate"
                        :md-disabled-dates="executedDisabledDates"
                        class="date-picker-custom"
                      >
                        <label>Executed Date</label>
                      </md-datepicker>
                      <span
                        v-if="!$v.transactionalForm.executed_date.required"
                        class="md-error"
                      >Executed Date is required</span>
                    </md-field>
                  </div>
                  <div class="md-layout-item md-small-size-100 md-size-50">
                    <md-field
                      slot="inputs"
                      class="multi-error datepicker-md-field"
                      :class="app.getValidationClass($v.transactionalForm,'closed_date')"
                    >
                      <!-- <md-input v-model="addForm.closed_date"></md-input> -->
                      <md-datepicker
                        v-model="transactionalForm.closed_date"
                        :md-disabled-dates="closedDisabledDates"
                        @md-opened="closedDateChange"
                        class="date-picker-custom"
                      >
                        <label>Closed Date</label>
                      </md-datepicker>
                      <span
                        v-if="!$v.transactionalForm.closed_date.required"
                        class="md-error"
                      >Closed Date is required</span>
                      <span
                        v-if="!$v.transactionalForm.closed_date.isExecutedDateSelect"
                        class="md-error"
                      >Select executed date first</span>
                    </md-field>
                  </div>
                  <div class="md-layout-item md-small-size-100 md-size-50">
                    <md-field slot="inputs">
                      <label>GCI</label>
                      <md-input v-model="transactionalForm.gci"></md-input>
                      <!-- <span v-if="!$v.userForm.role_id.required" class="md-error">Role is required</span> -->
                    </md-field>
                  </div>
                  <div class="md-layout-item md-small-size-100 md-size-50">
                    <md-field
                      slot="inputs"
                      :class="app.getValidationClass($v.transactionalForm,'additional_fees')"
                    >
                      <label>Additional fees</label>
                      <md-input v-model="transactionalForm.additional_fees"></md-input>
                      <span
                        v-if="!$v.transactionalForm.additional_fees.feesValid.isfloat"
                        class="md-error"
                      >Please enter decimal or number</span>
                    </md-field>
                  </div>
                </div>
                <md-dialog-actions class="flex-wrap px-0">
                  <div class="d-flex w-100 align-center justify-content-between nav-button-modal">
                    <div class="d-block text-center">
                      <md-button
                        type="submit"
                        class="cancel-link lighter-description mb-3 mt-3 md-hide"
                      >
                        <i class="fas fa-arrow-left mr-1"></i> Back
                      </md-button>
                    </div>
                    <div class="d-block text-center">
                      <md-button
                        :md-ripple="false"
                        type="submit"
                        class="md-primary w-100 button-primary-custom mb-3 mt-3 pl-3"
                        :class="[$v.transactionalForm.$invalid ? 'validate-error' : 'validate-active']"
                      >
                        <LoadingButtonLoader
                          :typeEnable="'simpleLoader'"
                          v-bind:enable="app.loading"
                          class="mr-1 next-button-outer"
                        />Next
                        <i class="fas fa-arrow-right ml-1"></i>
                      </md-button>
                    </div>
                  </div>
                </md-dialog-actions>
              </div>
            </div>
          </md-content>
        </form>
      </div>
    </md-dialog>

    <md-dialog :md-active.sync="openAddressDetail">
      <md-dialog-title class="modal-title d-block py-3 px-4 bg-light">
        <h4 class="my-0 md-headline text-left text-primary">Add Address</h4>
      </md-dialog-title>
      <div class="md-dialog-content">
        <form @submit.prevent="addAddressDetails">
          <md-content md-dynamic-height class="px-0">
            <div class="md-layout">
              <!-- <div class="md-layout-item md-medium-size-20 md-size-20 md-small-size-100">
                            <div class="modal-add-avatar d-flex flex-wrap justify-content-center mt-4 md-hide">
                                <div class="avatar-dialog d-flex align-center justify-content-center">
                                    <md-icon>person_outline</md-icon>
                                </div>
                                <span class="w-100 d-block brand-primary font-weight-regular description-normal text-center">Profile Avatar</span>
                            </div>
              </div>-->
              <div class="md-layout-item md-medium-size-80 md-size-80 md-small-size-100">
                <div class="md-layout">
                  <div class="md-layout-item md-small-size-100 md-size-50">
                    <md-field slot="inputs">
                      <label>Full name</label>
                      <md-input v-model="addForm.full_name"></md-input>
                      <!-- <span
                        v-if="!$v.userForm.first_name.required"
                        class="md-error"
                      >Firstname is required</span>-->
                    </md-field>
                  </div>
                  <div class="md-layout-item md-small-size-100 md-size-50">
                    <md-field slot="inputs">
                      <label>Email</label>
                      <md-input v-model="addForm.email"></md-input>
                      <!-- <span
                        v-if="!$v.userForm.first_name.required"
                        class="md-error"
                      >Firstname is required</span>-->
                    </md-field>
                  </div>
                  <div class="md-layout-item md-small-size-100 md-size-50">
                    <md-field slot="inputs">
                      <label>MLS no</label>
                      <!-- <md-input v-model="userForm.last_name"></md-input> -->
                      <md-input v-model="addForm.mls_no"></md-input>
                      <!-- <span
                        v-if="!$v.userForm.last_name.required"
                        class="md-error"
                      >Lastname is required</span>-->
                    </md-field>
                  </div>
                  <div class="md-layout-item md-small-size-100 md-size-50">
                    <md-field slot="inputs" class="multi-error">
                      <label>Street number</label>
                      <md-input v-model="addForm.street_number"></md-input>
                      <!-- <span v-if="!$v.userForm.email.required" class="md-error">Email is required</span>
                      <span v-if="!$v.userForm.email.email" class="md-error">Email is Invalid</span>-->
                    </md-field>
                  </div>
                  <div class="md-layout-item md-small-size-100 md-size-50">
                    <md-field slot="inputs" class="multi-error">
                      <label>Street name</label>
                      <md-input v-model="addForm.street_name"></md-input>
                      <!-- <span v-if="!$v.userForm.phone.required" class="md-error">Phone no is required</span>
                      <span
                        v-if="!$v.userForm.phone.phoneValid"
                        class="md-error"
                      >Phone no is Invalid</span>
                      <span v-if="!$v.userForm.phone.minLength" class="md-error">minLength is 10</span>
                      <span v-if="!$v.userForm.phone.maxLength" class="md-error">maxLength is 13</span>-->
                    </md-field>
                  </div>
                  <div class="md-layout-item md-small-size-100 md-size-50">
                    <md-field slot="inputs">
                      <label>City</label>
                      <md-input v-model="addForm.city"></md-input>
                      <!-- <md-select id="role" v-model="userForm.role_id">
                        <md-option value="2">Co-ordinator</md-option>
                        <md-option value="3">Agent</md-option>
                      </md-select>-->
                      <!-- <span v-if="!$v.userForm.role_id.required" class="md-error">Role is required</span> -->
                    </md-field>
                  </div>
                  <div class="md-layout-item md-small-size-100 md-size-50">
                    <md-field slot="inputs">
                      <label>State</label>
                      <md-input v-model="addForm.state"></md-input>
                      <!-- <span v-if="!$v.userForm.title.required" class="md-error">Title is required</span> -->
                    </md-field>
                  </div>
                  <div class="md-layout-item md-small-size-100 md-size-50">
                    <md-field slot="inputs">
                      <label>zip</label>
                      <!-- <md-input v-model="addForm.zip"></md-input> -->
                      <!-- <span v-if="!$v.userForm.title.required" class="md-error">Title is required</span> -->
                    </md-field>
                  </div>
                  <div class="md-layout-item md-small-size-100 md-size-50">
                    <md-field slot="inputs">
                      <label>country</label>
                      <!-- <md-input v-model="addForm.country"></md-input> -->
                      <!-- <span v-if="!$v.userForm.title.required" class="md-error">Title is required</span> -->
                    </md-field>
                  </div>
                </div>
                <md-dialog-actions class="flex-wrap justify-content-start px-0">
                  <md-button type="submit" class="md-primary button-primary-custom mb-3 mt-3 px-4">
                    <LoadingButtonLoader
                      :typeEnable="app.checkType('simpleLoader')"
                      v-bind:enable="app.loading"
                      class="mr-1"
                    />
                  </md-button>

                  <div class="d-block text-center">
                    <!-- <md-button
                      :md-ripple="false"
                      @click="showDialog = false"
                      class="cancel-link lighter-description mt-2 text-capitalize"
                    >Cancel</md-button>-->
                  </div>
                </md-dialog-actions>
              </div>
            </div>
          </md-content>
        </form>
      </div>
    </md-dialog>
    <md-dialog :md-active.sync="openCompleteStep" class="modal-medium">
      <md-dialog-title class="modal-title d-block py-3 px-4 bg-light position-relative">
        <h4 class="my-0 md-headline text-left text-primary">Complete Step</h4>
        <md-button @click="openCompleteStep = false" class="md-icon-button position-absolute">
          <md-icon class="text-primary">close</md-icon>
        </md-button>
      </md-dialog-title>
      <div class="md-dialog-content p-0">
        <md-content md-dynamic-height class="px-4">
          <div class="md-layout">
            <div
              class="md-layout-item md-small-size-100 md-size-100"
            >Your transactional is added successfully.</div>
            <md-dialog-actions class="flex-wrap justify-content-center">
              <md-button
                type="submit"
                @click="updateCompleteStep"
                class="md-primary w-100 button-primary-custom mb-2 mt-3"
              >
                <LoadingButtonLoader
                  :typeEnable="'simpleLoader'"
                  v-bind:enable="app.loading"
                  class="mr-1"
                />Done
              </md-button>
            </md-dialog-actions>
          </div>
        </md-content>
      </div>
    </md-dialog>
    <md-dialog :md-active.sync="editTransactionSteps">
      <md-dialog-title class="modal-title d-block py-3 px-4 bg-light position-relative">
        <h4 class="my-0 md-headline text-left text-primary">Edit Transaction</h4>
        <md-button @click="transactionSteps = false" class="md-icon-button position-absolute">
          <md-icon class="text-primary">close</md-icon>
        </md-button>
      </md-dialog-title>

      <div class="md-dialog-content p-0">
        <form @submit.prevent="updateTransactionDetails">
          <md-content md-dynamic-height class="px-4">
            <div class="md-layout">
              <div class="md-layout-item md-small-size-100 md-size-100">
                <md-field slot="inputs" :class="app.getValidationClass($v.editForm,'description')">
                  <label>Description</label>
                  <md-textarea v-model="editForm.description"></md-textarea>
                  <span
                    v-if="!$v.editForm.description.required"
                    class="md-error"
                  >Description is required</span>
                </md-field>
              </div>
              <div class="md-layout-item md-small-size-100 md-size-50">
                <md-field slot="inputs" :class="app.getValidationClass($v.editForm,'url')">
                  <label>URL (Vimeo, Youtube, ...)</label>
                  <md-input v-model="editForm.url"></md-input>
                  <span v-if="!$v.editForm.url.urlValid" class="md-error">Url invalid</span>
                </md-field>
              </div>

              <div class="md-layout-item md-small-size-100 md-size-50">
                <md-field slot="inputs">
                  <label>Transaction Type</label>

                  <md-select id="role" v-model="editForm.transaction_type">
                    <md-option value="1">Buyer</md-option>
                    <md-option value="2">Seller</md-option>
                  </md-select>
                  <!-- <span v-if="!$v.userForm.role_id.required" class="md-error">Role is required</span> -->
                </md-field>
              </div>

              <div class="md-layout-item md-small-size-100 md-size-50">
                <md-field slot="inputs">
                  <label>Sales price</label>
                  <span class="md-prefix">$</span>
                  <md-input v-model="editForm.sales_price"></md-input>
                  <!-- <span
                        v-if="!$v.userForm.first_name.required"
                        class="md-error"
                  >Firstname is required</span>-->
                </md-field>
              </div>

              <div class="md-layout-item md-small-size-100 md-size-50">
                <md-field slot="inputs">
                  <label>Additional fees</label>
                  <md-input v-model="editForm.additional_fees"></md-input>
                  <!-- <span
                        v-if="!$v.userForm.first_name.required"
                        class="md-error"
                  >Firstname is required</span>-->
                </md-field>
              </div>

              <div class="md-layout-item md-small-size-100 md-size-50">
                <md-field slot="inputs" class="multi-error">
                  <label>Executed Date</label>
                  <!-- <md-input v-model="editForm.executed_date"></md-input> -->

                  <md-datepicker v-model="editForm.executed_date" />
                  <div class="value">value: {{editForm.executed_date}}</div>
                  <!-- <span v-if="!$v.userForm.email.required" class="md-error">Email is required</span>
                  <span v-if="!$v.userForm.email.email" class="md-error">Email is Invalid</span>-->
                </md-field>
              </div>
              <div class="md-layout-item md-small-size-100 md-size-50">
                <md-field slot="inputs" class="multi-error">
                  <label>Closed Date</label>
                  <md-datepicker v-model="editForm.closed_date" />
                  <div class="value">value: {{editForm.closed_date}}</div>
                  <!-- <md-input v-model="editForm.closed_date"></md-input> -->
                  <!-- <span v-if="!$v.userForm.phone.required" class="md-error">Phone no is required</span>-->
                </md-field>
              </div>
              <div class="md-layout-item md-small-size-100 md-size-50">
                <md-field slot="inputs">
                  <label>GCI</label>
                  <md-input v-model="editForm.gci"></md-input>
                  <!-- <span v-if="!$v.userForm.role_id.required" class="md-error">Role is required</span> -->
                </md-field>
              </div>
              <div class="md-layout-item md-small-size-100 md-size-50">
                <md-field slot="inputs">
                  <label>Full Name</label>
                  <md-input v-model="editForm.full_name"></md-input>
                </md-field>
              </div>
              <div class="md-layout-item md-small-size-100 md-size-50">
                <md-field slot="inputs">
                  <label>Email</label>
                  <md-input v-model="editForm.email"></md-input>
                </md-field>
              </div>

              <div class="md-layout-item md-small-size-100 md-size-50">
                <md-field slot="inputs">
                  <label>MLS No</label>
                  <md-input v-model="editForm.mls_no"></md-input>
                </md-field>
              </div>

              <div class="md-layout-item md-small-size-100 md-size-50">
                <md-field slot="inputs">
                  <label>Street Number</label>
                  <md-input v-model="editForm.street_number"></md-input>
                </md-field>
              </div>

              <div class="md-layout-item md-small-size-100 md-size-50">
                <md-field slot="inputs">
                  <label>Street Name</label>
                  <md-input v-model="editForm.street_name"></md-input>
                </md-field>
              </div>

              <div class="md-layout-item md-small-size-100 md-size-50">
                <md-field slot="inputs">
                  <label>City</label>
                  <md-input v-model="editForm.city"></md-input>
                </md-field>
              </div>
              <div class="md-layout-item md-small-size-100 md-size-50">
                <md-field slot="inputs">
                  <label>State</label>
                  <md-input v-model="editForm.state"></md-input>
                  <!-- <span v-if="!$v.userForm.title.required" class="md-error">Title is required</span> -->
                </md-field>
              </div>
              <div class="md-layout-item md-small-size-100 md-size-50">
                <md-field slot="inputs">
                  <label>zip</label>
                  <md-input v-model="editForm.zip"></md-input>
                  <!-- <span v-if="!$v.userForm.title.required" class="md-error">Title is required</span> -->
                </md-field>
              </div>
              <div class="md-layout-item md-small-size-100 md-size-50">
                <md-field slot="inputs">
                  <label>country</label>
                  <md-input v-model="editForm.country"></md-input>
                  <!-- <span v-if="!$v.userForm.title.required" class="md-error">Title is required</span> -->
                </md-field>
              </div>
            </div>
          </md-content>
          <md-dialog-actions class="flex-wrap justify-content-center">
            <md-button type="submit" class="md-primary w-100 button-primary-custom mb-3 mt-3">Update</md-button>
            <!-- <div class="d-block w-100 text-center">
                    <md-button :md-ripple="false" @click="transactionSteps = false" class="cancel-link lighter-description mt-2 text-capitalize">skip</md-button>
            </div>-->
          </md-dialog-actions>
        </form>
      </div>
    </md-dialog>

    <div class="md-layout flex-md-column-reverse">
      <div class="md-layout-item md-medium-size-100 md-xsmall-size-100 md-size-75">
        <md-card class="md-card-plain md-card-custom custom-shadow radius-10 mt-0">
          <md-card-header class="px-4 mb-0 mt-0">
            <div class="md-headline d-flex align-center justify-content-between flex-wrap">
              <span class="text-primary">Transactions</span>
              <md-button
                class="md-primary button-custom-regular heading-add-button px-2"
                @click="openAddTransactionFunction"
              >
                <md-icon class="mr-1">add</md-icon>Add transaction
              </md-button>

              <!-- <md-button class="md-primary button-custom-regular heading-add-button" @click="openAddTransactionModal = true">
                            <md-icon class="mr-1">add</md-icon>Update public description
              </md-button>-->
            </div>
          </md-card-header>
          <md-card-content class="px-4 py-3 position-relative table-loader">
            <LoadingButtonLoader
              :typeEnable="'pageLoader'"
              v-bind:enable="app.loading"
              class="mr-1 table-loader position-absolute d-flex align-center justify-content-center w-100"
            />
            <md-table class="mb-3 transaction-table" md-sort="name" md-sort-order="asc">
              <md-table-row class="table-head">
                <md-table-head>TRANSACTION</md-table-head>
                <md-table-head>TRANSACTION NAME</md-table-head>
                <md-table-head>ADDRESS</md-table-head>
                <!-- <md-table-head>FIRSTNAME</md-table-head>
                <md-table-head>LASTNAME</md-table-head>-->
                <md-table-head>STATUS</md-table-head>
                <md-table-head>TYPE</md-table-head>
                <md-table-head>CREATED</md-table-head>
                <md-table-head>Agent(s)</md-table-head>
              </md-table-row>
              <md-table-row v-if="!transactions.length > 0">
                <md-table-cell colspan="7">
                  <!-- no data start -->
                  No record exist
                  <!-- no data end -->
                </md-table-cell>
              </md-table-row>
              <md-table-row v-for="transaction in transactions" v-bind:key="transaction.id">
                <md-table-cell>
                  <a :href="'transaction/'+transaction.id">#{{ transaction.transaction_code }}</a>
                </md-table-cell>
                <md-table-cell>{{ transaction.transaction_name }}</md-table-cell>
                <md-table-cell>{{ transaction.street_number }}{{ transaction.street_name }}{{transaction.city}}</md-table-cell>
                <!-- <md-table-cell>{{ transaction.first_name }}</md-table-cell>
                <md-table-cell>{{ transaction.last_name }}</md-table-cell>-->
                <md-table-cell v-if="transaction.status == 1">
                  <label
                    for
                    class="mr-3 active-label description d-flex align-center justify-content-center"
                  >{{ 'Active' }}</label>
                </md-table-cell>
                <md-table-cell v-else-if="transaction.status == 2">
                  <label
                    for
                    class="mr-3 closed-label description d-flex align-center justify-content-center"
                  >{{ 'Closed' }}</label>
                </md-table-cell>
                <md-table-cell v-else-if="transaction.status == 3">
                  <label
                    for
                    class="mr-3 closed-label description d-flex align-center justify-content-center"
                  >{{ 'Canceled' }}</label>
                </md-table-cell>
                <md-table-cell v-else>Not Active</md-table-cell>
                <md-table-cell v-if="transaction.transaction_type == 1">
                  <label
                    for
                    class="mr-3 active-label description d-flex align-center justify-content-center"
                  >{{ 'Buyer' }}</label>
                </md-table-cell>
                <md-table-cell v-else-if="transaction.transaction_type == 2">
                  <label
                    for
                    class="mr-3 closed-label description d-flex align-center justify-content-center"
                  >{{ 'Seller' }}</label>
                </md-table-cell>
                <md-table-cell>{{ transaction.created_at|formatDate }}</md-table-cell>
                <!-- <md-table-cell>{{ transaction.users }}</md-table-cell> -->

                <!-- <span v-for="tag in splitJoin(transaction.images)" v-text="tag"></span> -->

                <!-- <md-table-cell
                  v-for="tag in splitJoin(transaction.users,transaction.images)"
                  v-bind:key="tag.id"
                >{{ tag }}</md-table-cell>-->

                <md-table-cell>
                  <md-list class="agents-ul">
                    <md-list-item
                      v-for="(index, item ) in calculateImageDetail(transaction.users,transaction.images)"
                      v-bind:key="item"
                    >
                      <div class="md-avatar" v-if="index">
                        <img v-bind:src="'/images/users/'+index" :alt="item" />
                      </div>
                      <div class="md-avatar md-initial" v-else>
                        <span>{{ item }}</span>
                      </div>
                    </md-list-item>
                  </md-list>
                </md-table-cell>
                <!-- <span v-for="tag in splitJoin(object.tags)" v-text="tag"></span> -->
                <!-- <md-table-cell>Image</md-table-cell> -->
                <md-table-cell md-label class="md-hide">
                  <md-menu md-direction="bottom-end">
                    <md-button md-menu-trigger class="md-icon-button icon-button-flat">
                      <md-icon>more_vert</md-icon>
                    </md-button>
                    <md-menu-content class="menu-content-custom">
                      <!-- <md-menu-item @click="editTransaction(transaction.id,transaction.user_id)">Edit</md-menu-item> -->
                      <!-- <md-menu-item @click="deleteUser(user.id)">Delete</md-menu-item> -->
                    </md-menu-content>
                  </md-menu>
                </md-table-cell>
              </md-table-row>
            </md-table>
            <div class="md-layout" v-if="totalTransactions > 10">
              <div
                class="md-layout-item md-medium-size-50 md-xsmall-size-100 md-size-50 d-flex align-center"
              >
                <p
                  class="description font-weight-medium"
                  v-if="totalTransactions"
                >Displaying {{ currentCount }} of {{ totalTransactions }} records</p>
              </div>
              <div class="md-layout-item md-medium-size-50 md-xsmall-size-100 md-size-50">
                <!-- for pagination you can check vue material kit -->
                <ul class="pagination">
                  <li
                    class="page-item prev-page"
                    v-if="page != 1"
                    v-bind:class="[changePage == pageNumber ? 'active' : '']"
                  >
                    <a @click="page--" class="page-link" aria-label="Previous">
                      <i class="fas fa-angle-double-left"></i>
                      <span v-if="withText" class="pr-1">Prev</span>
                    </a>
                  </li>
                  <li
                    class="page-item"
                    v-for="pageNumber in pages.slice(page-1, page+5)"
                    v-bind:key="pageNumber"
                    v-bind:class="[changePage == pageNumber ? 'active' : '']"
                  >
                    <a @click="page = pageNumber" class="page-link">{{ pageNumber }}</a>
                  </li>

                  <li
                    class="page-item page-pre next-page"
                    v-if="page < pages.length"
                    v-bind:class="[changePage == pageNumber ? 'active' : '']"
                  >
                    <a @click="page++" class="page-link" aria-label="Next">
                      <span class="pl-1">Next page</span>
                      <i class="fas fa-angle-double-right"></i>
                    </a>
                  </li>

                  <span class="md-hide">{{ changePage }}</span>
                </ul>
              </div>
            </div>
          </md-card-content>
        </md-card>
      </div>
      <div class="md-layout-item md-medium-size-100 md-xsmall-size-100 md-size-25">
        <div class="search-right mb-4">
          <md-field
            class="custom-shadow bg-white search-outer p-3 radius-10 position-relative"
            md-inline
          >
            <img :src="'../img/search-icon.svg'" alt style="width: 20px;" class="position-absolute" />
            <label>Search</label>
            <md-input v-on:keyup.native="doFilter" v-model="search_filter" class="search-input"></md-input>
          </md-field>
          <div class="mx-auto filter-section-outer filter-transaction">
            <div class="d-flex justify-content-between align-center mb-3 filter-heading-section">
              <h4 class="md-headline text-primary font-weight-medium my-0 hide-md">Filter</h4>
              <div class="d-flex justify-content-between">
                <span :class="{toggled: isToggled}" @click="isToggled = !isToggled">
                  <md-icon class="display-md" style="cursor:pointer">filter_list</md-icon>
                </span>
                <a @click="resetFilter" class="ml-2">
                  <small class="lighter-description font-weight-medium">Reset filter</small>
                </a>
              </div>

              <!-- <a :href="href" @click="navigate">Reset Filter</a>
              <router-link to="/transactions">-->
              <!-- <router-link to="/transactions2"><small class="lighter-description font-weight-medium">Reset filter</small></router-link> -->
            </div>
            <div class="filter-section" v-if="isToggled">
              <div>
                <div class="d-flex justify-content-start">
                  <md-icon class="text-primary">date_range</md-icon>
                  <h4 class="description text-primary w-100 pl-2">Created date</h4>
                </div>
                <div class="d-flex justify-content-between align-center">
                  <v-md-date-range-picker
                    :opens="'right'"
                    :presets="presets"
                    @change="dateFilterChange"
                    class="custom-daterange-picker"
                  ></v-md-date-range-picker>
                  <!-- <tags-input element-id="tags" :placeholder="'Transaction Template'" v-model="selected_date" :existing-tags="existingDateTags" :typeahead="true" :typeahead-style="typeaheadStyle" :allow-duplicates="allowDuplicates" :typeaheadHideDiscard="typeaheadHideDiscard" @tag-added="onDateTagAdded"></tags-input> -->
                  <!-- <p class="brand-primary font-weight-semi-bold mb-0 pl-4">Aug 01, 2019 - Dec 22, 2019</p>
                  <md-icon class="cursor-pointer closed-label mr-3 closed-label close-label-circle description d-flex align-center justify-content-center">close</md-icon>-->
                </div>
              </div>
              <div>
                <div class="d-flex justify-content-start">
                  <md-icon class="text-primary">inbox</md-icon>
                  <h4 class="description text-primary w-100 pl-2">Status</h4>
                </div>
                <div class="d-flex justify-content-between align-center">
                  <md-checkbox
                    v-model="status_filter"
                    :value="1"
                    @change="handleChangeFilter($event)"
                    class
                  >Active</md-checkbox>
                  <label
                    for
                    class="label-account"
                    v-if="countArray[0]"
                  >{{ countArray[0].activeCount}}</label>
                </div>
                <div class="d-flex justify-content-between align-center">
                  <md-checkbox
                    v-model="status_filter"
                    :value="2"
                    @change="handleChangeFilter($event)"
                    class
                  >Closed</md-checkbox>
                  <label
                    for
                    class="label-account"
                    v-if="countArray[0]"
                  >{{ countArray[0].closedCount}}</label>
                </div>
                <div class="d-flex justify-content-between align-center">
                  <md-checkbox
                    v-model="status_filter"
                    :value="3"
                    @change="handleChangeFilter($event)"
                    class
                  >Canceled</md-checkbox>
                  <label
                    for
                    class="label-account"
                    v-if="countArray[0]"
                  >{{ countArray[0].canceledCount}}</label>
                </div>
                <!-- <input type="checkbox" id="jack" value="Jack" v-model="checkedNames">
                            <input type="checkbox" id="john" value="John" v-model="checkedNames">
                            <input type="checkbox" id="mike" value="Mike" v-model="checkedNames">
                <label for="mike">Mike</label>-->
                <hr />
              </div>
              <div>
                <div class="d-flex justify-content-start">
                  <!-- <md-icon class="text-primary">people_alt</md-icon> -->
                  <md-icon class="text-primary">inbox</md-icon>
                  <h4 class="description text-primary w-100 pl-2">Type</h4>
                </div>
                <div class="d-flex justify-content-between align-center">
                  <md-checkbox
                    v-model="type_filter"
                    :value="1"
                    @change="handleChangeFilter($event)"
                    class
                  >Buyer</md-checkbox>
                  <label
                    for
                    class="label-account"
                    v-if="countArray[0]"
                  >{{ countArray[0].buyerCount}}</label>
                </div>
                <div class="d-flex justify-content-between align-center">
                  <md-checkbox
                    v-model="type_filter"
                    :value="2"
                    @change="handleChangeFilter($event)"
                    class
                  >Seller</md-checkbox>
                  <label
                    for
                    class="label-account"
                    v-if="countArray[0]"
                  >{{ countArray[0].sellerCount}}</label>
                </div>
                <hr />
              </div>

              <div>
                <div class="d-flex justify-content-start">
                  <md-icon class="text-primary">people_alt</md-icon>
                  <h4 class="description text-primary w-100 pl-2">By Role</h4>
                </div>
                <div class="d-flex justify-content-between align-center">
                  <md-checkbox
                    v-model="role_filter"
                    :value="2"
                    @change="handleChangeFilter($event)"
                    class
                  >Agent</md-checkbox>
                  <label
                    for
                    class="label-account"
                    v-if="countArray[0]"
                  >{{ countArray[0].agentCount }}</label>
                </div>
                <div class="d-flex justify-content-between align-center">
                  <md-checkbox
                    v-model="role_filter"
                    :value="3"
                    @change="handleChangeFilter($event)"
                    class
                  >Co-ordinator</md-checkbox>
                  <label
                    for
                    class="label-account"
                    v-if="countArray[0]"
                  >{{ countArray[0].coordinatorCount }}</label>
                </div>
                <!-- <div class="d-flex justify-content-between align-center">
                                <md-checkbox
                                    v-model="role_filter"
                                    :value="5"
                                    @change="handleChangeFilter($event)"
                                    class
                                >Transaction User</md-checkbox>
                                <label for class="label-account">35</label>
                </div>-->
                <hr />
              </div>
              <div>
                <div class="d-flex justify-content-start">
                  <md-icon class="text-primary">star</md-icon>
                  <h4 class="description text-primary w-100 pl-2">By Rating</h4>
                </div>
                <div class="d-flex justify-content-between align-center">
                  <span>
                    <!-- <md-checkbox v-model="checkbox7" class>
                                        1
                                        <md-icon class="star"></md-icon>
                                    </md-checkbox>
                                    <md-checkbox v-model="checkbox8" class>
                                        2
                                        <md-icon class="star"></md-icon>
                                    </md-checkbox>
                                    <md-checkbox v-model="checkbox9" class>
                                        3
                                        <md-icon class="star"></md-icon>
                                    </md-checkbox>
                                    <md-checkbox v-model="checkbox10" class>
                                        4
                                        <md-icon class="star"></md-icon>
                                    </md-checkbox>
                                    <md-checkbox v-model="checkbox11" class>
                                        5
                                        <md-icon class="star"></md-icon>
                    </md-checkbox>-->
                  </span>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <!-- {{ countArray }} -->
    <!-- <md-button class="md-primary md-icon-button" @click="editTransaction">
        <md-icon>edit</md-icon>
    </md-button>-->

    <md-button class="md-primary md-hide" @click="openAddTransactionUserModal">
      <md-icon>add</md-icon>
    </md-button>

    <md-button class="md-primary md-hide" @click="testOpenTransactionSteps">
      <md-icon>add</md-icon>
    </md-button>

    <md-button class="md-primary md-hide" @click="openAddNewStepModal = true">
      <md-icon>add</md-icon>Add new step
    </md-button>

    <md-button class="md-primary md-hide" @click="openAddNewUser = true">
      <md-icon>add</md-icon>Add new user
    </md-button>
    <!-- <TransactionUsers></TransactionUsers> -->

    <!--Add transaction User Modal-->
    <AddTransactionUserModal
      :active="openAddTransactionUserModal"
      :app="app"
      :transaction_id="passTransactionId()"
      @dialogClose="handleDialogClose"
      @dialogOpen="handleDialogOpen"
      ref="addTransactionUserModal"
    />

    <TransactionStepsModal
      :active="openAddTransactionSteps"
      :app="app"
      :transaction_id="passTransactionId()"
      @dialogClose="handleDialogClose"
      @dialogOpen="handleDialogOpen"
      ref="transactionStepsModal"
      :templates="existingTags"
    />
    <AddNewStepModal
      :active="openAddNewStepModal"
      :app="app"
      :transaction_id="passTransactionId()"
      @dialogClose="handleDialogClose"
      @dialogOpen="handleDialogOpen"
      ref="addNewStepModal"
    />

    <AddUserModel
      :active="openAddNewUser"
      :app="app"
      :userModelTitle="'Add New User'"
      :propObj1="false"
      :propObj2="''"
      :transaction_id="passTransactionId()"
      @dialogClose="handleDialogClose"
      @dialogOpen="handleDialogOpen"
      ref="addUserModel"
    />
  </div>
</template>

<script>
import { HTTP } from "../../httpCommon";
import draggable from "vuedraggable";
import {
  required,
  email,
  minLength,
  maxLength,
  sameAs
} from "vuelidate/lib/validators";

import LoadingButtonLoader from "../../components/LoadingButtonLoader";
import TransactionUsers from "./reuseComponents/TransactionUsers";
import TransactionStepsModal from "./reuseComponents/TransactionStepsModal";
import AddNewStepModal from "./reuseComponents/AddNewStepModal";
import AddTransactionUserModal from "./reuseComponents/AddTransactionUserModal";
import AddUserModel from "./reuseComponents/AddUserModel";

import VoerroTagsInput from "@voerro/vue-tagsinput";

const isUrl = value =>
  /^(|https?:\/\/[\w\-_]+(\.[\w\-_]+)+([\w\-\.,@?^=%&amp;:/~\+#]*[\w\-\@?^=%&amp;/~\+#])?)$/.test(
    value
  ); //phone valid

const isFloat = value => /^[+-]?\d+(\.\d+)?$/.test(value); //phone valid

// import parse from "date-fns/parse";
// import format from "date-fns/format";

export default {
  name: "Transaction",
  props: ["app"],
  components: {
    LoadingButtonLoader,
    draggable,
    TransactionStepsModal,
    AddNewStepModal,
    AddTransactionUserModal,
    AddUserModel,
    "tags-input": VoerroTagsInput
  },
  // props: ['steps'],
  data: function() {
    // let dateFormat = this.$material.locale.dateFormat || "yyyy/MM/dd";
    let now = new Date();
    return {
      openAddTransactionModal: false,
      openTransactionDetails: false,
      openAddTransactionUserModal: false, //started Add transactional users
      openAddNewStepModal: false,
      openAddNewUser: false,
      editTransactionSteps: false,
      openAddTransactionSteps: false,
      date: Number(now),
      addForm: {
        transactional_id: "",
        transaction_name: "",
        transaction_type: ""
        // first_name: "",
        // last_name: "",
        // description: "",
        // url: "",
        // source: "",
      },
      transactionalForm: {
        sales_price: "",
        seller_assist: "",
        street_no: "",
        street_address: "",
        city: "",
        state: "",
        zip_code: "",
        mls_no: "",
        executed_date: "",
        closed_date: "",
        gci: "",
        additional_fees: ""
      },
      editForm: {
        description: "",
        url: "",
        sales_price: "",
        transaction_type: "",
        additional_fees: "",
        executed_date: null,
        closed_date: "",
        gci: "",
        agent_review_url: "",
        full_name: "",
        email: "",
        mls_no: "",
        street_number: "",
        street_name: "",
        city: "",
        state: "",
        zip: "",
        country: ""
      },
      openAddressDetail: false,
      openCompleteStep: false,
      transactions: "",
      addAddress: false,
      transactionEditId: "",
      transactionUserId: "",
      transactionAutoIncrementId: "",
      checkedSteps: [],
      selectedSteps: [],
      transactionUser: {
        firstName: "",
        lastName: "",
        emailAddress: "",
        phone: "",
        accessCode: "",
        emailSend: ""
      },
      openAddStep: false,
      editStatus: false,
      transactionsTypesList: "",
      //tags plugin info start
      selectedTransactionTags: [],
      templates: [],
      existingTags: [],
      allowDuplicates: false,
      typeaheadStyle: "dropdown",
      typeaheadHideDiscard: true,
      citiesLists: [],
      stateLists: [],
      executedDisabledDates: date => {
        return new Date() > date;
      },
      closedDisabledDates: date => {
        return new Date() > date;
      },
      searchParam: {
        search: "",
        created_date: "",
        status: "",
        type: "",
        role: ""
      },
      search_filter: "",
      status_filter: [],
      type_filter: [],
      role_filter: [],
      isToggled: true,
      countArray: [],
      totalTransactions: "",
      calling: false,
      /*pagination variables*/
      page: 1, //current page
      perPage: 10,
      pages: [],
      currentCount: "",
      pageNumber: "",
      withText: Boolean,
      /*date-range picker */
      presets: [],
      dateRange1: "",
      dateRange2: "",
      tagSearchResult: false
    };
  },
  validations: {
    addForm: {
      // first_name: {
      //     required
      // },
      // last_name: {
      //     required
      // },
      // description: {
      //     required,
      //     maxLength: maxLength(300)
      // },
      // url: {
      //     urlValid: isUrl
      // },
      transaction_name: {
        required
      },
      transaction_type: {
        required
      }
    },
    transactionalForm: {
      sales_price: {
        required,
        priceValid: isFloat
      },
      street_no: {
        required
      },
      street_address: {
        required
      },
      state: {
        required
      },
      city: {
        required,
        isStateSelect(value) {
          if (!this.transactionalForm.state) {
            return false;
          } else {
            return true;
          }
        }
      },
      executed_date: {
        required
      },
      closed_date: {
        required,
        isExecutedDateSelect(value) {
          if (!this.transactionalForm.executed_date) {
            return false;
          } else {
            return true;
          }
        }
      },
      additional_fees: {
        feesValid: isFloat
      }
    },
    editForm: {
      description: {
        required
      },
      url: {
        urlValid: isUrl
      }
    }
    // email: {
    //     required,
    //     email
    // },
    // phone: {
    //     required,
    //     phoneValid: isPhone,
    //     minLength: minLength(10),
    //     maxLength: maxLength(13)
    // },
    // role_id: {
    //     required
    // },
    // title: {
    //     required
    // }
  },
  created() {
    this.init();
  },
  methods: {
    init() {
      this.getAllTransactions();
      HTTP.get("api/transactions/getAllTransactionsTypes")
        .then(response => {
          if (response.data.Status) {
            this.transactionsTypesList = response.data.TransactionsTypes;
          }
        })
        .catch(error => {
          // this.app.loading = false;
        });

      HTTP.get("api/templates/getAllCompanyTemplates")
        .then(response => {
          if (response.data.Status) {
            this.templates = response.data.CompanyTemplates;

            this.existingTags = this.templates.map(item => {
              return {
                key: item.steps_lists,
                value: item.template_name
              };
            });
          }
        })
        .catch(error => {
          // this.app.loading = false;
        });
    },
    splitJoin(users, images) {
      let obj = [];
      if (users) {
        if (users.indexOf(",") > -1) {
          let userArray = users.split(",");
        } else {
          obj[users] = images;
        }
      }
      return obj;
    },
    getAllTransactions(from = 0, to = 10, calling = false) {
      console.log(from);
      console.log("main-filter");
      this.app.loading = true;
      HTTP.get("api/transactions/getAllTransactions", {
        params: {
          from: from,
          to: to
        }
      })
        .then(response => {
          if (response.data.Status) {
            this.app.loading = false;
            this.transactions = response.data.Transactions;
            this.currentCount = this.transactions.length;
            this.countArray = response.data.Count;
            this.totalTransactions = this.countArray[0].rowCount;

            if (!calling) {
              this.setPages(); //set pages count
            }
          }
        })
        .catch(error => {
          this.app.loading = false;
        });
    },
    // getExecutedDates() {
    //     HTTP.get("api/transactions/getExecutedDates")
    //         .then(response => {
    //             if (response.data.Status) {
    //                 //this.existingDateTags = response.data.Dates;
    //             }
    //         })
    //         .catch(error => {

    //         });
    // },
    doFilter(extra_param = null, from = 0, to = 10) {
      // console.log("do-filter");

      this.app.loading = true;
      HTTP.get("api/transactions/getAllTransactions", {
        params: {
          from: from,
          to: to,
          search_filter: this.search_filter,
          status_filter: this.status_filter,
          type_filter: this.type_filter,
          role_filter: this.role_filter,
          dateRange1: this.dateRange1 ? this.dateRange1 : null,
          dateRange2: this.dateRange1 ? this.dateRange2 : null
        }
      })
        .then(response => {
          if (response.data.Status) {
            this.app.loading = false;
            this.transactions = response.data.Transactions;
            this.currentCount = this.transactions.length;
            this.countArray = response.data.Count;
            this.totalTransactions = this.countArray[0].rowCount;
          }
        })
        .catch(error => {
          this.app.loading = false;
        });
    },
    handleChangeFilter(event) {
      this.doFilter();
    },
    openAddTransactionFunction() {
      this.selectedTransactionTags = [];
      this.addForm = {
        transactional_id: "",
        transaction_name: "",
        transaction_type: ""
      };

      this.$v.addForm.$reset();
      let current_time = new Date();
      let transactional_string = current_time.valueOf().toString();
      this.addForm.transactional_id = transactional_string.substring(5);
      this.openAddTransactionModal = true;
    },
    openTransactionDetailsFunction() {
      this.transactionalForm = {
        sales_price: "",
        seller_assist: "",
        street_no: "",
        street_address: "",
        city: "",
        state: "",
        zip_code: "",
        mls_no: "",
        executed_date: "",
        closed_date: "",
        gci: "",
        additional_fees: ""
      };
      this.$v.transactionalForm.$reset();
      this.openAddTransactionModal = false; //close previous popup
      HTTP.get("api/transactions/getAllStates").then(response => {
        if (response.data.Status) {
          this.stateLists = response.data.States;
        }
      });
      this.openTransactionDetails = true;
    },
    openAddTransactionUserFunction() {
      this.openTransactionDetails = false;
      (this.openAddTransactionUserModal = true), //started Add transactional users = true;
        // this.$refs.AddTransactionUserModal.init();
        //initiate child addTransactionUserModal component
        this.$refs.addTransactionUserModal.init(true);
    },
    testOpenTransactionSteps() {
      this.openAddTransactionSteps = true;
      this.$refs.transactionStepsModal.init(true);
    },
    addTransaction() {
      //add transactions
      this.$v.addForm.$touch();
      // console.log(this.$v.addForm.$error);
      if (this.$v.addForm.$error) return;

      const data = {
        transactional_id: this.addForm.transactional_id,
        transaction_name: this.addForm.transaction_name,
        transaction_type: this.addForm.transaction_type,
        paramType: "template", //Add template first time
        steps: this.selectedTransactionTags
      };

      this.app.loading = true;
      HTTP.post("api/transactions/addTransaction", data)
        .then(response => {
          if (response.data.Status) {
            this.app.loading = false;
            this.transactionAutoIncrementId = response.data.ResultId;
            this.openTransactionDetailsFunction(); //Transaction details dialog
            this.$toasted
              .show(response.data.Message, {
                type: "success"
                // icon: "error_outline"
              })
              .goAway(1500);
          }
        })
        .catch(error => {
          this.app.loading = false;
        });
    },
    addTransactionalDetails() {
      //add transactional details
      this.$v.transactionalForm.$touch();
      console.log(this.$v.transactionalForm.$error);
      //   if (this.$v.transactionalForm.$error) return;

      if (this.$v.transactionalForm.$error) {
        if (
          this.$v.transactionalForm.sales_price.$error ||
          this.$v.transactionalForm.street_no.$error ||
          this.$v.transactionalForm.street_address.$error ||
          this.$v.transactionalForm.state.$error ||
          this.$v.transactionalForm.city.$error
        ) {
          var myDiv = document.getElementById("transaction-detail");
          myDiv.scrollTop = 0;
        }

        //window.scrollTo(0,0);
        return;
      }

      const data = {
        transactionAutoIncrementId: this.transactionAutoIncrementId
          ? this.transactionAutoIncrementId
          : null,
        sales_price: this.transactionalForm.sales_price,
        seller_assist: this.transactionalForm.seller_assist,
        street_no: this.transactionalForm.street_no,
        street_address: this.transactionalForm.street_address,
        state: this.transactionalForm.state,
        city: this.transactionalForm.city
          ? JSON.parse(this.transactionalForm.city).id
          : null,
        zip_code: this.transactionalForm.zip_code,
        mls_no: this.transactionalForm.mls_no,
        executed_date: this.transactionalForm.executed_date,
        strto_executed_date: this.transactionalForm.executed_date
          ? Number(new Date(this.transactionalForm.executed_date))
          : null,
        closed_date: this.transactionalForm.closed_date,
        strto_closed_date: this.transactionalForm.closed_date
          ? Number(new Date(this.transactionalForm.closed_date))
          : null,
        gci: this.transactionalForm.gci,
        additional_fees: this.transactionalForm.additional_fees
      };
      this.app.loading = true;
      HTTP.put("api/transactions/addTransactionDetails", data)
        .then(response => {
          if (response.data.Status) {
            this.app.loading = false;
            this.transactionAutoIncrementId = response.data.ResultId;
            this.openAddTransactionUserFunction(); //Address Dialog for subscription user
            // this.getAllTransactions();
          }
        })
        .catch(error => {
          this.app.loading = false;
        });
    },
    addAddressDetails() {
      //add Address Details
      const data = {
        transactionAutoIncrementId: this.transactionAutoIncrementId,
        full_name: this.addForm.full_name,
        email: this.addForm.email,
        mls_no: this.addForm.mls_no,
        street_number: this.street_number,
        street_name: this.street_name,
        city: this.addForm.city,
        state: this.addForm.state,
        zip: this.addForm.zip,
        country: this.addForm.country
      };

      this.app.loading = true;
      HTTP.put("api/transactions/addUsersAddress", data)
        .then(response => {
          if (response.data.Status) {
            this.app.loading = false;
            this.openAddTransactionUserModal = true; //Address Dialog for subscription user
            this.transactionAutoIncrementId = response.data.ResultId;
          }
        })
        .catch(error => {
          this.app.loading = false;
        });
    },
    addTransactionUsers() {
      const data = {
        transactionAutoIncrementId: this.transactionAutoIncrementId,
        firstName: this.transactionUser.firstName,
        lastName: this.transactionUser.lastName,
        emailAddress: this.transactionUser.emailAddress,
        phone: this.transactionUser.phone,
        accessCode: this.transactionUser.accessCode,
        emailSend: this.transactionUser.emailSend,
        allocated_steps: this.selectedSteps
      };

      HTTP.put("api/transactions/addTransactionUsers", data)
        .then(response => {
          if (response.data.Status) {
            this.app.loading = false;
            this.transactionAutoIncrementId = response.data.ResultId;
            this.openCompleteStep = true; //Address Dialog for subscription user
          }
        })
        .catch(error => {
          this.app.loading = false;
        });
    },
    updateCompleteStep() {
      this.openCompleteStep = false;
      this.getAllTransactions("", "", true);
      // const data = {
      //     transactionAutoIncrementId: this.transactionAutoIncrementId
      // }

      // HTTP.put("api/transactions/updateCompleteStep", data)
      //     .then(response => {
      //         if (response.data.Status) {
      //             this.app.loading = false;
      //             if (response.data.Result) {
      //                 this.openCompleteStep = false;
      //                 this.transactionAutoIncrementId = "";
      //                 this.$toasted
      //                     .show(response.data.Message, {
      //                         type: "success"
      //                         // icon: "error_outline"
      //                     })
      //                     .goAway(1500);
      //             }
      //         }
      //     })
      //     .catch(error => {
      //         this.app.loading = false;
      //     });
    },
    editTransaction(id, transactionUserId) {
      console.log(id);
      this.transactionEditId = id;
      this.transactionUserId = transactionUserId;
      this.editTransactionSteps = true;
      let findIndex = this.transactions
        .map((item, v) => {
          return item.id;
        })
        .indexOf(id);

      let transactionDetail = this.transactions[findIndex];

      this.editForm.description = transactionDetail.description;
      this.editForm.url = transactionDetail.url;
      this.editForm.sales_price = transactionDetail.sales_price;
      this.editForm.additional_fees = transactionDetail.additional_fees;
      if (transactionDetail.executed_date) {
        this.editForm.executed_date = Number(transactionDetail.executed_date);
      } else {
        this.editForm.executed_date = null;
      }

      if (transactionDetail.closed_date) {
        this.editForm.closed_date = Number(transactionDetail.closed_date);
      } else {
        this.editForm.closed_date = null;
      }

      this.editForm.gci = transactionDetail.gci;
      this.editForm.agent_review_url = transactionDetail.agent_review_url;
      // debugger;
      // this.transactions.map(()=>{

      // })
    },
    updateTransactionDetails() {
      this.$v.editForm.$touch();
      if (this.$v.editForm.$error) return;
      const data = {
        transactionEditId: this.editForm.transactionEditId,
        transactionUserId: this.editForm.transactionUserId,
        transaction_type: this.editForm.transaction_type,
        sales_price: this.editForm.sales_price,
        additional_fees: this.editForm.additional_fees,
        executed_date: this.editForm.executed_date
          ? Number(this.editForm.executed_date)
          : null,
        closed_date: this.editForm.closed_date
          ? Number(this.editForm.executed_date)
          : null,
        gci: this.editForm.gci,
        agent_review_url: this.editForm.agent_review_url,
        full_name: this.editForm.full_name,
        email: this.editForm.email,
        mls_no: this.editForm.mls_no,
        street_number: this.editForm.street_number,
        street_name: this.editForm.street_name,
        city: this.editForm.city,
        state: this.editForm.state,
        zip: this.editForm.zip,
        country: this.editForm.country
      };

      HTTP.put("api/transactions/updateTransaction", data)
        .then(response => {
          if (response.data.Status) {
            this.app.loading = false;
            // this.addAddress = true;
            this.int();
            this.$toasted
              .show(response.data.Message, {
                type: "success"
                // icon: "error_outline"
              })
              .goAway(1500);
          }
        })
        .catch(error => {
          this.app.loading = false;
        });
    },
    pickerChange(item) {
      console.log(item);
    },
    getSteps() {
      HTTP.get("api/steps/getTemplateSteps")
        .then(response => {
          if (response.data.Status) {
            this.activeSteps = response.data.Steps;
            let activeSteps = this.activeSteps;

            this.checkedSteps = this.activeSteps.map((item, i) => {
              return {
                id: item.id,
                steps_title: item.steps_title,
                checked: false,
                order: i
              };
            });
          }
        })
        .catch(error => {});
    },
    onStart(evt) {
      console.log(evt.oldIndex);
    },
    move(arr, old_index, new_index) {
      while (old_index < 0) {
        old_index += arr.length;
      }
      while (new_index < 0) {
        new_index += arr.length;
      }
      if (new_index >= arr.length) {
        var k = new_index - arr.length;
        while (k-- + 1) {
          arr.push(undefined);
        }
      }
      arr.splice(new_index, 0, arr.splice(old_index, 1)[0]);
      return arr;
    },
    onEnd: function(/**Event*/ evt) {
      var itemEl = evt.item; // dragged HTMLElement
      evt.to; // target list
      evt.from; // previous list
      evt.oldIndex; // element's old index within old parent
      evt.newIndex; // element's new index within new parent
      evt.oldDraggableIndex; // element's old index within old parent, only counting draggable elements
      evt.newDraggableIndex; // element's new index within new parent, only counting draggable elements
      evt.clone; // the clone element
      evt.pullMode; // when item is in another sortable: `"clone"` if cloning, `true` if moving

      //debugger;
      console.log(evt.oldIndex);

      console.log(evt.newIndex);

      // console.log(this.myArray);
      // console.log(this.move(this.myArray, 0, 2));
    },
    dragSelection() {
      // console.log(this.checkedNames);
      // console.log(this.activeSteps);

      //For Checked selection
      let removeCheckedArray = this.checkedSteps.map((item, i) => {
        if (item.checked) {
          console.log(i);
          this.selectedSteps.push({
            id: item.id,
            steps_title: item.steps_title,
            checked: false
          });

          return true;
        }
      });

      // console.log(removeCheckedArray);
      //Go through removeValFromIndex in reverse order and you can .splice() without messing up the indexes of the yet-to-be-removed items.
      //removeValFromIndex.sort(function(a,b){ return b - a; });
      for (var i = removeCheckedArray.length - 1; i >= 0; i--) {
        if (removeCheckedArray[i]) {
          this.checkedSteps.splice(i, 1); //Remove from checkedSteps
        }
      }

      //for selected
      let selectedCheckedArray = this.selectedSteps.map((item, i) => {
        if (item.checked) {
          this.checkedSteps.push({
            id: item.id,
            steps_title: item.steps_title,
            checked: false
          });
          //this.selectedSteps.splice(i, 1);
          return true;
        }
      });

      //Loop in reverse order
      for (var i = selectedCheckedArray.length - 1; i >= 0; i--) {
        if (selectedCheckedArray[i]) {
          this.selectedSteps.splice(i, 1); //Remove from selectedSteps
        }
      }
    },
    handleDialogOpen(event) {
      if (event == "add-steps") {
        //assign steps
        this.openAddTransactionUserModal = false;
        this.openAddTransactionSteps = true;
        this.$refs.transactionStepsModal.init(true);
      }
      if (event == "steps-assigned-success") {
        this.openAddTransactionSteps = false;
        this.openCompleteStep = true;
      }
      if (event == "add-new-step") {
        this.$refs.addNewStepModal.init(true);
        this.openAddNewStepModal = true;
      }

      if (event == "add-user") {
        this.openAddNewUser = true;
        this.$refs.addUserModel.init(false); //initiate empty values--------------------
      }
    },
    handleDialogClose(event) {
      if (event == "add-steps") {
        this.openAddStep = false;
        this.openAddNewStepModal = false;
      }
      // console.log('data after child handle: ', event) // get the data after child dealing
      if (event == "transaction-steps") {
        this.openAddTransactionSteps = false;
      }

      if (event == "add-transaction-user") {
        this.openAddTransactionUserModal = false;
      }

      if (event == "add-new-step") {
        this.openAddNewStepModal = false;
      }

      if (event == "add-user") {
        this.openAddNewUser = false;
      }
      if (event == "transaction-steps-open-transaction-users") {
        //open to rea
        this.openAddStep = false;
        this.openAddTransactionSteps = false;
        this.openAddTransactionUserModal = true;
      }
    },
    passTransactionId() {
      //Pass transaction Id to child components on basis of edits and add
      if (!this.editStatus) {
        return this.transactionAutoIncrementId;
      } else {
        return this.transactionEditId;
      }
    },
    onTagAdded(slug) {
      console.log(`Tag added: ${slug}`);
    },
    citySelect(value) {
      this.$v.transactionalForm.city.$touch();
      // HTTP.get("api/transactions/getAllStates", {
      //     params: {
      //         id_state: value.id_state
      //     }
      // }).then(response => {
      //     if (response.data.Status) {
      //         this.stateLists = response.data.States;
      //     }
      // });
    },
    stateSelect(value) {
      this.citiesLists = [];
      this.transactionalForm.city = [];
      this.app.loading2 = true;
      HTTP.get("api/transactions/getAllCities", {
        params: {
          id_state: value
        }
      }).then(response => {
        if (response.data.Status) {
          this.app.loading2 = false;
          this.citiesLists = response.data.Cities;
        }
      });
    },
    executedDateChange(event) {},
    changeDate(event) {
      console.log("executed date closed---");
      this.transactionalForm.closed_date = "";
      // if (this.transactionalForm.executed_date) {
      //     console.log(this.transactionalForm.executed_date);
      //     this.transactionalForm.closed_date = "";
      //     this.closedDateFocus = true;
      //     this.closedDisabledDates = date => {
      //         let date2 = new Date(this.transactionalForm.executed_date);

      //         // Add a day
      //         date2.setDate(date2.getDate() + 1);

      //         return date2 > date;
      //     };
      // }
      // debugger;
    },
    closedDateChange(event) {
      if (!this.transactionalForm.executed_date) {
        // this.transactionalForm.closed_date = "";
        this.$v.transactionalForm.closed_date.$touch();
      }
    },
    getInitials(string) {
      var names = string.split(" "),
        initials = names[0].substring(0, 1).toUpperCase();

      if (names.length > 1) {
        initials += names[names.length - 1].substring(0, 1).toUpperCase();
      }
      //console.log(initials);
      return initials;
    },
    splitJoin(theText) {
      return theText.split(", ");
    },
    calculateImageDetail(users, images) {
      let obj = {};

      //First check users is undefined or not null
      if (typeof users != "undefined" && users != null) {
        // let user_count = users.split(", ");

        if (users.indexOf(",") > -1) {
          let userArray = users.split(",");
          // console.log(userArray);
          //Next check images is undefined or not null
          if (typeof images != "undefined" && images != null) {
            let imagesArray = images.split(",");
            let imageArrayCount = 5; //@S@SOFTO : 20 MARCH as we will show limited user

            for (let i = 0; i < 5; i++) {
              if (userArray[i]) {
                let initials = this.getInitials(userArray[i]);

                obj[initials] = imagesArray[i];
              }
            }
          }
        } else {
          let initials = this.getInitials(users);
          obj[initials] = images;
        }
      }

      return obj;
    },
    resetFilter() {
      this.search_filter = "";
      this.status_filter = [];
      this.type_filter = [];
      this.role_filter = [];
      this.getAllTransactions((calling = true));
    },
    setPages() {
      let numberOfPages = Math.ceil(this.totalTransactions / this.perPage);
      for (let index = 1; index <= numberOfPages; index++) {
        this.pages.push(index);
      }
      // debugger;
    },
    paginate() {
      //console.log("paginate");
      let page = this.page;
      let perPage = this.perPage;
      let from = page * perPage - perPage;
      let to = page * perPage;
      // return posts.slice(from, to);

      if (
        this.search_filter ||
        this.status_filter ||
        this.type_filter ||
        this.role_filter
      ) {
        let extra_param = null;
        this.doFilter(extra_param, from, to);
      } else {
        this.getAllTransactions(from, to);
      }
    },
    dateFilterChange(values) {
      // console.log(values);
      let date1 = values[0].toDate();
      let date2 = values[1].toDate();
      this.dateRange1 =
        date1.getFullYear() + "-" + date1.getMonth() + "-" + date1.getDate();
      this.dateRange2 =
        date2.getFullYear() + "-" + date2.getMonth() + "-" + date2.getDate();

      // console.log("date fileter chnge");
      // console.log(date1.valueOf());
      // console.log(date2.valueOf());

      // this.doFilter();

      if (date2) {
        if (date1.valueOf() !== date2.valueOf()) {
          //console.log("not matched");
          //this.doFilter();
        }
      }
    },
    onTagChange(search) {
      if (this.templates.length) {
        let existingTags = this.templates
          .map(item => {
            var re = new RegExp(search, "gi");
            return item.template_name.match(re);
          })
          .filter(function(el) {
            return el != null;
          });

        //  console.log(existingTags);
        if (!existingTags.length) {
          this.tagSearchResult = true;
        } else {
          this.tagSearchResult = false;
        }
      } else {
        this.tagSearchResult = false;
      }
    }
  },
  computed: {
    changePage() {
      //  console.log("computed execution---");
      this.paginate();
      return this.page;
    }
  },
  watch: {
    page(newVal, oldVal) {
      console.log(
        "The old value of length was: " +
          oldVal +
          "\nThe new value of length is: " +
          newVal
      );
    }
  }
};
</script>

<style scoped>
</style>
