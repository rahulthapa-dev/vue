<template>
  <div>
    <!-- TRANSACTION BASIC DETAILS -->
    <md-dialog :md-active.sync="publicDetails" class="modal-medium">
      <md-dialog-title class="modal-title d-block py-3 px-4 bg-light position-relative">
        <h4 class="my-0 md-headline text-left text-primary">Update public description</h4>
        <md-button @click="publicDetails = false" class="md-icon-button position-absolute">
          <md-icon class="text-primary">close</md-icon>
        </md-button>
      </md-dialog-title>
      <div class="md-dialog-content p-0">
        <form @submit.prevent="updateProductDetails">
          <md-content md-dynamic-height class="px-4">
            <div class="md-layout">
              <div class="md-layout-item md-small-size-100 md-size-100">
                <md-field
                  slot="inputs"
                  :class="app.getValidationClass($v.productForm,'description')"
                >
                  <label>Description</label>
                  <md-textarea
                    v-model="productForm.description"
                    class="pl-0"
                    value="At vero eos et accusamus et iusto odio dignissimos ducimus qui blanditiis praesentium voluptatum deleniti atque corrupti quos dolores et quas molestias."
                  ></md-textarea>
                  <!-- ERROR MESSAGE -->
                  <span
                    v-if="!$v.productForm.description.required"
                    class="md-error"
                  >Description is required</span>
                </md-field>
              </div>
              <div class="md-layout-item md-small-size-100 md-size-100">
                <md-field slot="inputs">
                  <label>URL (Vimeo, Youtube, ...)</label>
                  <md-input v-model="productForm.url"></md-input>
                </md-field>
              </div>
            </div>
          </md-content>
          <md-dialog-actions class="flex-wrap justify-content-center">
            <md-button type="submit" class="md-primary w-100 button-primary-custom mb-2 mt-3">
              Update
              <LoadingButtonLoader
                :typeEnable="'simpleLoader'"
                v-bind:enable="app.loading"
                class="mr-1"
              />
            </md-button>
            <div class="d-block w-100 text-center">
              <md-button
                :md-ripple="false"
                @click="publicDetails = false"
                class="cancel-link lighter-description mt-2 mb-3 text-capitalize"
              >Cancel</md-button>
            </div>
          </md-dialog-actions>
        </form>
      </div>
    </md-dialog>
    <!-- CLOSE TRANSACTION -->
    <md-dialog :md-active.sync="closeTransaction" class="modal-medium">
      <md-dialog-title class="modal-title d-block py-3 px-4 bg-light position-relative">
        <h4 class="my-0 md-headline text-left text-primary">Close transaction</h4>
        <md-button @click="closeTransaction = false" class="md-icon-button position-absolute">
          <md-icon class="text-primary">close</md-icon>
        </md-button>
      </md-dialog-title>
      <div class="md-dialog-content p-0">
        <form>
          <md-content md-dynamic-height class="px-4">
            <div class="md-layout">
              <div class="md-layout-item md-size-100">
                <p class="description mt-4 mb-3">
                  All users moved to
                  <span class="brand-primary">'Review Email'</span> will receive a review email.
                </p>
              </div>
            </div>
            <div class="md-layout mb-3">
              <div class="md-layout-item md-size-45 md-medium-size-45 md-small-size-100">
                <h4 class="md-title text-left text-primary">No review email</h4>
                <md-list class="list-add-template">
                  <md-list-item class="mb-2">
                    <input type="checkbox" />
                    <label class="d-flex justify-content-between align-center">
                      <div
                        class="position-relative flex-wrap justify-content-start d-flex align-center"
                      >
                        <div class="md-avatar position-absolute">AB</div>
                        <p class="md-title text-primary">Step 1</p>
                      </div>
                      <i class="fas fa-angle-double-right description pr-3"></i>
                    </label>
                  </md-list-item>
                </md-list>
              </div>
              <div class="d-flex align-center justify-content-center mx-auto outer-swap-wrapper">
                <div class="outer-swap">
                  <div class="inner-swap">
                    <i class="fas fa-angle-double-left text-primary"></i>
                    <i class="fas fa-angle-double-right text-primary"></i>
                  </div>
                </div>
              </div>
              <div class="md-layout-item md-size-45 md-medium-size-45 md-small-size-100">
                <h4 class="md-title text-left text-primary">Receive review email</h4>
                <md-list class="list-add-template assigned-list">
                  <md-list-item class="mb-2">
                    <input type="checkbox" />
                    <label class="d-flex justify-content-between align-center">
                      <div
                        class="position-relative flex-wrap justify-content-start d-flex align-center"
                      >
                        <div class="md-avatar position-absolute">AB</div>
                        <p class="md-title text-primary">Step 3</p>
                      </div>
                      <i class="fas fa-angle-double-left description pr-3"></i>
                    </label>
                  </md-list-item>
                </md-list>
              </div>
            </div>
          </md-content>
          <md-dialog-actions class="flex-wrap justify-content-center">
            <md-button
              class="md-danger w-100 button-primary-custom mb-1 mt-3"
              @click="closeTransaction = false"
            >Close</md-button>
            <div class="d-block w-100 text-center">
              <md-button
                :md-ripple="false"
                @click="closeTransaction = false"
                class="cancel-link lighter-description mt-2 mb-3 text-capitalize"
              >Cancel</md-button>
            </div>
          </md-dialog-actions>
        </form>
      </div>
    </md-dialog>
    <!-- UPDATE TRANSACTION DETAILS MODEL -->
    <md-dialog :md-active.sync="overviewEdit" class="modal-medium">
      <md-dialog-title class="modal-title d-block py-3 px-4 bg-light position-relative">
        <h4 class="my-0 md-headline text-left text-primary">Update transaction details</h4>
        <md-button @click="overviewEdit = false" class="md-icon-button position-absolute">
          <md-icon class="text-primary">close</md-icon>
        </md-button>
      </md-dialog-title>
      <div class="md-dialog-content p-0">
        <form @submit.prevent="updateProductTransactionDetails">
          <md-content md-dynamic-height class="px-4">
            <div class="md-layout">
              <div class="md-layout-item md-small-size-100 md-size-50">
                <md-field
                  slot="inputs"
                  :class="app.getValidationClass($v.productTransactionForm,'sales_price')"
                >
                  <label>Sales price</label>
                  <span class="md-prefix">$</span>
                  <md-input value="120,000" v-model="productTransactionForm.sales_price"></md-input>
                  <span
                    v-if="!$v.productTransactionForm.sales_price.required"
                    class="md-error"
                  >Sales price is required</span>
                </md-field>
              </div>
              <div class="md-layout-item md-small-size-100 md-size-50">
                <md-field
                  slot="inputs"
                  :class="app.getValidationClass($v.productTransactionForm,'additional_fee')"
                >
                  <label>Additional fees</label>
                  <md-input v-model="productTransactionForm.additional_fee"></md-input>
                  <span
                    v-if="!$v.productTransactionForm.additional_fee.required"
                    class="md-error"
                  >Additional price is required</span>
                </md-field>
              </div>
              <div class="md-layout-item md-small-size-100 md-size-50">
                <md-field
                  slot="inputs"
                  :class="app.getValidationClass($v.productTransactionForm,'execuate_date')"
                  class="multi-error datepicker-md-field"
                >
                  <md-datepicker
                    v-model="productTransactionForm.execuate_date"
                    class="date-picker-custom"
                  >
                    <label>Execute date</label>
                  </md-datepicker>
                  <span
                    v-if="!$v.productTransactionForm.execuate_date.required"
                    class="md-error"
                  >Executed price is required</span>
                </md-field>
              </div>
              <div class="md-layout-item md-small-size-100 md-size-50">
                <md-field
                  slot="inputs"
                  :class="app.getValidationClass($v.productTransactionForm,'closed_date')"
                  class="multi-error datepicker-md-field"
                >
                  <md-datepicker
                    v-model="productTransactionForm.closed_date"
                    class="date-picker-custom"
                  >
                    <label>Closed dates</label>
                  </md-datepicker>
                  <span
                    v-if="!$v.productTransactionForm.closed_date.required"
                    class="md-error"
                  >Executed price is required</span>
                </md-field>
              </div>
              <div class="md-layout-item md-small-size-100 md-size-50">
                <md-field slot="inputs">
                  <label>GCI</label>
                  <md-input v-model="productTransactionForm.gci"></md-input>
                </md-field>
              </div>
            </div>
          </md-content>
          <md-dialog-actions class="flex-wrap justify-content-center">
            <md-button type="submit" class="md-primary w-100 button-primary-custom mb-1 mt-3">
              Update
              <LoadingButtonLoader
                :typeEnable="'simpleLoader'"
                v-bind:enable="app.loading"
                class="mr-1"
              />
            </md-button>
            <div class="d-block w-100 text-center">
              <md-button
                :md-ripple="false"
                @click="overviewEdit = false"
                class="cancel-link lighter-description mt-2 mb-3 text-capitalize"
              >Cancel</md-button>
            </div>
          </md-dialog-actions>
        </form>
      </div>
    </md-dialog>
    <!-- UPDATE USER DETAILS -->
    <md-dialog :md-active.sync="userEdit" class="modal-medium">
      <md-dialog-title class="modal-title d-block py-3 px-4 bg-light position-relative">
        <h4 class="my-0 md-headline text-left text-primary">Update details</h4>
        <md-button @click="userEdit = false" class="md-icon-button position-absolute">
          <md-icon class="text-primary">close</md-icon>
        </md-button>
      </md-dialog-title>
      <div class="md-dialog-content p-0">
        <form @submit.prevent="updateUserDetails">
          <md-content md-dynamic-height class="px-4">
            <div class="md-layout">
              <!-- FIRST NAME AND LAST NAME -->
              <div class="md-layout-item md-small-size-100 md-size-50">
                <md-field
                  slot="inputs"
                  :class="app.getValidationClass($v.userDetailsForm,'first_name')"
                >
                  <label>First name</label>
                  <md-input value="120,000" v-model="userDetailsForm.first_name"></md-input>
                  <span
                    v-if="!$v.userDetailsForm.first_name.required"
                    class="md-error"
                  >First name is required</span>
                </md-field>
              </div>
              <div class="md-layout-item md-small-size-100 md-size-50">
                <md-field
                  slot="inputs"
                  :class="app.getValidationClass($v.userDetailsForm,'last_name')"
                >
                  <label>Last name</label>
                  <md-input value="120,000" v-model="userDetailsForm.last_name"></md-input>
                  <span
                    v-if="!$v.userDetailsForm.last_name.required"
                    class="md-error"
                  >Last name is required</span>
                </md-field>
              </div>
              <!-- STREET NO AND STREET ADDRESS -->
              <div class="md-layout-item md-small-size-100 md-size-50">
                <md-field
                  slot="inputs"
                  :class="app.getValidationClass($v.userDetailsForm,'street_no')"
                >
                  <label>Street no</label>
                  <md-input v-model="userDetailsForm.street_no"></md-input>
                  <span
                    v-if="!$v.userDetailsForm.street_no.required"
                    class="md-error"
                  >Last name is required</span>
                </md-field>
              </div>
              <div class="md-layout-item md-small-size-100 md-size-50">
                <md-field
                  slot="inputs"
                  :class="app.getValidationClass($v.userDetailsForm,'street_address')"
                >
                  <label>Street address</label>
                  <md-input v-model="userDetailsForm.street_address"></md-input>
                  <span
                    v-if="!$v.userDetailsForm.street_address.required"
                    class="md-error"
                  >Last name is required</span>
                </md-field>
              </div>
              <!-- CITY AND STATE -->
              <div class="md-layout-item md-small-size-100 md-size-50">
                <md-field slot="inputs" :class="app.getValidationClass($v.userDetailsForm,'state')">
                  <label>Select State</label>
                  <md-select id="role" v-model="userDetailsForm.state" @md-selected="stateSelect">
                    <md-option
                      :value="item.id"
                      v-for="(item) in stateLists"
                      :key="item.id"
                    >{{ item.state_name }}</md-option>
                  </md-select>
                  <span v-if="!$v.userDetailsForm.state.required" class="md-error">State is required</span>
                </md-field>
              </div>
              <div class="md-layout-item md-small-size-100 md-size-50">
                <md-field slot="inputs" :class="app.getValidationClass($v.userDetailsForm,'city')">
                  <label>Select city</label>
                  <md-select
                    id="addForm-city"
                    v-model="userDetailsForm.city"
                    @md-selected="citySelect"
                  >
                    <md-option
                      :value="JSON.stringify({ id: item.id, id_state: item.id_state })"
                      v-for="(item) in citiesLists"
                      :key="item.id"
                    >{{ item.city }}</md-option>
                  </md-select>
                  <span v-if="!$v.userDetailsForm.city.required" class="md-error">City is required</span>
                </md-field>
              </div>
              <!-- ZIP CODE AND MLS -->
              <div class="md-layout-item md-small-size-100 md-size-50">
                <md-field slot="inputs">
                  <label>MLS no.</label>
                  <md-input v-model="userDetailsForm.mls_no"></md-input>
                </md-field>
              </div>
              <div class="md-layout-item md-small-size-100 md-size-50">
                <md-field slot="inputs">
                  <label>Zip</label>
                  <md-input v-model="userDetailsForm.zip_code"></md-input>
                </md-field>
              </div>
            </div>
          </md-content>
          <md-dialog-actions class="flex-wrap justify-content-center">
            <md-button type="submit" class="md-primary w-100 button-primary-custom mb-1 mt-3">
              <LoadingButtonLoader
                :typeEnable="'simpleLoader'"
                v-bind:enable="app.loading"
                class="mr-1"
              />Update
            </md-button>
            <div class="d-block w-100 text-center">
              <md-button
                :md-ripple="false"
                @click="userEdit = false"
                class="cancel-link lighter-description mt-2 mb-3 text-capitalize"
              >Cancel</md-button>
            </div>
          </md-dialog-actions>
        </form>
      </div>
    </md-dialog>
    <!-- Transaction list detail -->
    <div class="md-layout flex-md-column-reverse">
      <div class="md-layout-item md-medium-size-100 md-xsmall-size-100 md-size-75">
        <md-card class="md-card-plain md-card-custom custom-shadow radius-10 mt-0">
          <md-card-header class="px-4 mb-0 mt-0">
            <div class="md-headline d-flex align-center justify-content-between">
              <!-- TRANSACTION STATUS  1: Active , 2: Closed , 3: Inactive-->
              <span class="brand-primary">{{transaction.transaction_code}}</span>
              <label
                v-if="transaction.status ==1"
                class="mr-3 active-label description d-flex align-center justify-content-center"
              >Active</label>

              <label
                v-if="transaction.status ==2"
                class="mr-3 closed-label description d-flex align-center justify-content-center"
              >Closed</label>
              <label
                v-if="transaction.status ==0"
                class="mr-3 archived-label description d-flex align-center justify-content-center"
              >Inactive</label>
            </div>
          </md-card-header>
          <md-card-content class="px-4 py-3">
            <!-- Public details start -->
            <section class="mb-5">
              <div class="md-layout flex-xsmall-column-reverse">
                <div
                  class="md-layout-item md-size-60 md-medium-size-60 md-small-size-50 md-xsmall-size-100"
                >
                  <div class="d-flex align-center mb-3">
                    <h4 class="md-title text-primary mr-3 font-weight-medium">Public Details</h4>
                    <span
                      class="cursor-pointer brand-primary"
                      @click="edit('product-basic-details')"
                    >Edit</span>
                  </div>
                  <p class="text-primary description">{{transaction.description}}</p>
                </div>
                <div
                  class="md-layout-item md-size-40 md-medium-size-40 md-small-size-50 md-xsmall-size-100"
                >
                  <div style="--aspect-ratio: 16/9;">
                    <iframe
                      v-if="transaction.url"
                      width="560"
                      height="315"
                      :src="videoId"
                      frameborder="0"
                      allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture"
                      allowfullscreen
                    ></iframe>
                  </div>
                </div>
              </div>
            </section>
            <!-- Public details end -->
            <!-- AGENTS SECTION-->
            <div class="example">
              <section class="mb-5">
                <div class="md-layout">
                  <div class="md-layout-item d-flex justify-content-between mb-3">
                    <h3 class="md-title text-primary">Agent(s)</h3>
                    <!-- <md-button class="md-primary md-icon-button" @click="openAddAgentDialog = true">
                      <md-icon>add</md-icon>
                    </md-button>-->

                    <md-button
                      class="md-primary md-icon-button"
                      @click="openAddUserDialogF('add-agent')"
                    >
                      <md-icon>add</md-icon>
                    </md-button>
                  </div>
                </div>
                <!-- AGENT SECTION-->
                <TransactionAgents
                  :transaction_id="transaction_id"
                  :app="app"
                  @dialogClose="handleDialogClose"
                  ref="transactionAgents"
                ></TransactionAgents>

                <section id="app" class="section">
                  <div class="md-layout">
                    <div class="md-layout-item d-flex justify-content-between mb-3">
                      <h3 class="md-title text-primary">Transaction Steps</h3>
                      <div class="d-flex align-center">
                        <p
                          class="text-primary mr-3 mb-0 cursor-pointer"
                          @click="importTemplate = true"
                        >
                          <!-- <md-icon class="text-primary mr-1">get_app</md-icon>Import Template -->
                        </p>
                        <md-button class="md-primary md-icon-button" @click="openStep()">
                          <md-icon>add</md-icon>
                        </md-button>
                      </div>
                    </div>
                  </div>

                  <TransactionSteps
                    :app="app"
                    :transaction_id="transaction_id"
                    @dialogOpen="handleDialogOpen"
                    @dialogClose="handleDialogClose"
                    ref="transactionSteps"
                  ></TransactionSteps>
                </section>
                <!-- TRANSACTIONS USERS -->
                <div class="md-layout">
                  <div class="md-layout-item d-flex justify-content-between mb-3">
                    <h3 class="md-title text-primary">Transaction Users</h3>

                    <md-button
                      class="md-primary md-icon-button"
                      @click="openAddUserDialogF('add-transaction-user')"
                    >
                      <md-icon>add</md-icon>
                    </md-button>
                  </div>
                </div>
                <TransactionUsers @dialogClose="handleDialogClose"></TransactionUsers>

                <AddTransactionUserModal
                  @dialogOpen="handleDialogOpen"
                  :active="openTUserDialog"
                  :app="app"
                  @dialogClose="handleDialogClose"
                  :transaction_id="passTransactionId()"
                  ref="addTransactionUserModel"
                />

                <!-- ADD AGENT MODEL -->

                <AddUserModel
                  :userModelTitle="userModelTitle"
                  :propObj="app"
                  :propObj1="commissionField"
                  :propObj2="modelType"
                  :propObj3="userCompanyField"
                  :active="openAddUserDialog"
                  :app="app"
                  @dialogClose="handleDialogClose"
                  :transaction_id="passTransactionId()"
                  ref="addUserModel"
                />
                <AddTransactionStepModal
                  :app="app"
                  :active="openAddTransactionStepDialog"
                  @dialogOpen="handleDialogOpen"
                  @dialogClose="handleDialogClose"
                  :transaction_id="passTransactionId()"
                  ref="addTransactionStepModal"
                />

                <AddNewStepModal
                  :active="openStepDialog"
                  :app="app"
                  @dialogClose="handleDialogClose"
                  :transaction_id="transaction_id"
                  ref="addNewStepModel"
                />
                <!-- STEPS SECTION-->
                <!-- VENDOR SECTION -->
                <div class="md-layout">
                  <div class="md-layout-item d-flex justify-content-between mb-3">
                    <h3 class="md-title text-primary">Vendor(s)</h3>
                    <md-button
                      class="md-primary md-icon-button"
                      @click="openAddUserDialogF('add-vendor')"
                    >
                      <md-icon>add</md-icon>
                    </md-button>
                  </div>
                </div>
                <TransactionVendors
                  :propObj="app"
                  :app="app"
                  ref="transactionVendors"
                  @dialogOpen="handleDialogOpen"
                ></TransactionVendors>
              </section>
            </div>
          </md-card-content>
        </md-card>
      </div>
      <!-- RIGHT SIDE -->
      <div class="md-layout-item md-medium-size-100 md-xsmall-size-100 md-size-25">
        <div class="search-right mb-4 search-right-transaction-detail">
          <div class="mx-auto filter-section-outer mb-4">
            <div class="mb-3 filter-icon-button">
              <md-button v-if="this.refreshButton" class="md-secondary md-icon-button ml-0">
                <md-icon class="fa-spin">autorenew</md-icon>
              </md-button>

              <md-button class="md-white md-icon-button ml-0" @click="refreshTransaction()" v-else>
                <md-icon class>autorenew</md-icon>
              </md-button>
              <md-button class="md-white md-icon-button" @click="closeTransaction = true">
                <md-icon>close</md-icon>
              </md-button>
            </div>
            <div class="d-flex justify-content-between align-end mb-3 filter-heading-section">
              <h4 class="md-headline text-primary font-weight-medium my-0">Overview</h4>
              <small
                class="brand-primary font-weight-medium description-normal cursor-pointer"
                @click="edit('product-transaction-details')"
              >Edit</small>
            </div>
            <div class="md-layout">
              <div class="md-layout-item md-size-50 mb-2">
                <label for class="description description-normal">Transaction ID</label>
                <p class="text-primary description">{{transaction.transaction_code}}</p>
              </div>
              <div class="md-layout-item md-size-50 mb-2">
                <label for class="description description-normal">Transaction Type</label>
                <p class="text-primary description" v-if="transaction.name">{{transaction.name}}</p>
                <p class="brand-primary description" v-else>N/A</p>
              </div>
              <div class="md-layout-item md-size-50 mb-2">
                <label for class="description description-normal">Sales Price</label>
                <p
                  class="brand-primary md-title"
                  v-if="transaction.sales_price"
                >${{transaction.sales_price}}</p>
                <p class="brand-primary md-title" v-else>N/A</p>
              </div>
              <div class="md-layout-item md-size-50 mb-2">
                <label for class="description description-normal">Additional Fees</label>
                <p
                  class="text-primary description"
                  v-if="transaction.additional_fees"
                >${{transaction.additional_fees}}</p>
                <p class="brand-primary description" v-else>N/A</p>
              </div>
              <div class="md-layout-item md-size-50 mb-2">
                <label for class="description description-normal">Closed Dates</label>
                <p
                  class="text-primary description"
                  v-if="transaction.closed_date"
                >{{transaction.closed_date | formatDate}}</p>
                <p class="brand-primary description" v-else>N/A</p>
              </div>
              <div class="md-layout-item md-size-50 mb-2">
                <label for class="description description-normal">Executed Date</label>
                <p
                  class="text-primary description"
                  v-if="transaction.executed_date"
                >{{transaction.executed_date | formatDate}}</p>
                <p class="brand-primary description" v-else>N/A</p>
              </div>
              <div class="md-layout-item md-size-50 mb-2">
                <label for class="description description-normal">Sources</label>
                <p class="text-success description">
                  <md-icon class="font-11">fiber_manual_record</md-icon>
                </p>
              </div>
            </div>
          </div>
          <div class="mx-auto">
            <div class="d-flex justify-content-between align-end mb-3 filter-heading-section">
              <h4 class="md-headline text-primary font-weight-medium my-0">Details</h4>
              <small
                class="brand-primary font-weight-medium description-normal cursor-pointer"
                @click="edit('user-details')"
              >Edit</small>
            </div>
            <div class="md-card md-card-custom custom-shadow radius-10 mt-0 overflow-hidden mb-0">
              <div class="md-card-content">
                <div class="md-layout">
                  <!-- <div class="md-layout-item md-size-100 mb-2">
                    <label for class="description description-normal">Full Name</label>
                    <p class="text-primary description">{{fullName}}</p>
                  </div> -->
                  <div class="md-layout-item md-size-100 mb-2">
                    <label for class="description description-normal">MLS #</label>
                    <p class="text-primary description">{{transaction.mls_no}}</p>
                  </div>
                  <div class="md-layout-item md-size-100 mb-2">
                    <label for class="description description-normal">Address</label>
                    <p class="text-primary description">
                      {{transaction.street_no}}
                      {{transaction.street_address}}
                      <br />
                      <span v-if="transaction.city">{{transaction.city}},</span>
                      {{transaction.state_code}}
                      <br />
                      {{transaction.county}} {{transaction.zip_code}}
                    </p>
                  </div>
                  <div class="md-layout-item md-size-100 mb-2">
                    <label for class="description description-normal">
                      Review URL
                      <md-icon></md-icon>
                    </label>
                    <p class="brand-primary description">Here company url wil come</p>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import { HTTP } from "../../httpCommon";
import TransactionAgents from "./TransactionAgents";
import TransactionVendors from "./TransactionVendors";
import AddUserModel from "./reuseComponents/AddUserModel";
import AddTransactionStepModal from "./reuseComponents/TransactionStepsModal";

import TransactionSteps from "./TransactionSteps";
import AddNewStepModal from "./reuseComponents/AddNewStepModal";

import TransactionUsers from "./TransactionUsers";
import AddTransactionUserModal from "./reuseComponents/AddTransactionUserModal";

import AddAgentModel from "./reuseComponents/AddAgentModel";
import AddVendorModel from "./reuseComponents/AddVendorModel";
import LoadingButtonLoader from "../../components/LoadingButtonLoader";

import {
  required,
  email,
  minLength,
  maxLength,
  sameAs
} from "vuelidate/lib/validators";

export default {
  props: ["app"],
  name: "SingleTransaction",
  components: {
    TransactionAgents,
    TransactionUsers,
    TransactionSteps,
    AddUserModel,
    AddAgentModel,
    TransactionVendors,
    AddVendorModel,
    AddNewStepModal,
    AddTransactionUserModal,
    AddTransactionStepModal,
    LoadingButtonLoader
  },
  data: function() {
    return {
      userModelTitle: "Add agent", //sending to child component

      openAddAgentDialog: false,
      openAddVendorDialog: false,
      openAddUserDialog: false,
      commissionField: true,
      userCompanyField: false,
      modelType: "", //By Default nothing
      transaction: "",
      publicDetails: false,
      overviewEdit: false,
      openStepDialog: false,
      openUserDialog: false,
      openTUserDialog: false,
      transaction_id: "",
      videoId: "",
      closeTransaction: false,
      openAddTransactionStepDialog: false,
      userEdit: false,
      url: "",
      description: "",
      citiesLists: [],
      stateLists: [],
      refreshButton: false,
      productForm: {
        url: "",
        description: ""
      },
      productTransactionForm: {
        sales_price: "",
        additional_fee: "",
        execuate_date: "",
        closed_date: "",
        gci: ""
      },
      userDetailsForm: {
        first_name: "",
        last_name: "",
        city: "",
        state: "",
        street_no: "",
        street_address: "",
        zip_code: "",
        mls_no: ""
      }
    };
  },
  mounted() {
    this.init();
  },
  methods: {
    init() {
      this.transaction_id = this.$route.params.id;
      this.getTransaction(); //Calling transaction details
    },
    refreshTransaction() {
      this.getTransaction();
    },
    passTransactionId() {
      return this.$route.params.id;
    },
    citySelect(value) {
      HTTP.get("api/transactions/getAllStates", {
        params: {
          id_state: value.id_state
        }
      }).then(response => {
        if (response.data.Status) {
          this.stateLists = response.data.States;
        }
      });
    },
    stateSelect(value) {
      console.log("select state");
      this.getAllCities(value);
    },
    /**
     * Update User details realted to transaction right section
     *
     */
    updateUserDetails() {
      console.log("submit here");
      event.preventDefault();
      this.$v.userDetailsForm.$touch();
      if (this.$v.userDetailsForm.$error) return;
      console.log("I am here");

      var data = this.userDetailsForm;
      data.type = "user-details";
      data.transaction_id = this.transaction_id;

      this.app.loading = true;
      this.updateProduct(data);
    },
    /**
     * Generate you tube embed link
     */
    edit(param) {
      if (param == "product-basic-details") {
        this.publicDetails = true;
        this.productForm = {
          url: this.transaction.url,
          description: this.transaction.description
        };
      } else if (param == "product-transaction-details") {
        //OPEN OVERVIEW MODEL
        this.overviewEdit = true;
        this.productTransactionForm = {
          sales_price: this.transaction.sales_price,
          additional_fee: this.transaction.additional_fees,
          execuate_date: this.transaction.executed_date,
          closed_date: this.transaction.closed_date,
          gci: this.transaction.gci
        };
      } else if (param == "user-details") {
        this.userEdit = true;
        //OPEN USER DETAILS MODEL
        this.getAllStates();
        this.getAllCities(this.transaction.state);

        this.userDetailsForm = {
          first_name: this.transaction.first_name,
          last_name: this.transaction.last_name,
          city: this.transaction.city_code,
          state: this.transaction.state,
          street_no: this.transaction.street_no,
          street_address: this.transaction.street_address,
          zip_code: this.transaction.zip_code,
          mls_no: this.transaction.mls_no
        };
      }
    },
    getAllCities(value) {
      HTTP.get("api/transactions/getAllCities", {
        params: {
          id_state: value
        }
      }).then(response => {
        if (response.data.Status) {
          this.citiesLists = response.data.Cities;
        }
      });
    },
    getAllStates() {
      HTTP.get("api/transactions/getAllStates").then(response => {
        if (response.data.Status) {
          this.stateLists = response.data.States;
        }
      });
    },
    updateProduct(data) {
      HTTP.post("api/transactions/updateProduct", data)
        .then(response => {
          if (response.data.Status) {
            this.refreshTransaction();
            this.$toasted
              .show(response.data.Message, {
                type: "success"
              })
              .goAway(1500);
            this.app.loading = false;
            this.publicDetails = false;
            this.userEdit = false;
            this.close();
          }
        })
        .catch(error => {});
    },
    /**
     * Update Product Detais
     */
    updateProductDetails() {
      event.preventDefault();
      this.$v.productForm.$touch(); //do not remove this
      if (this.$v.productForm.$error) return;

      var data = this.productForm;
      data.type = "basic-details";
      data.transaction_id = this.transaction_id;

      this.app.loading = true;
      this.updateProduct(data);
    },
    updateProductTransactionDetails() {
      event.preventDefault();
      this.$v.productTransactionForm.$touch(); //do not remove this
      console.log("details");
      if (this.$v.productTransactionForm.$error) return;
      console.log("here ");
      var data = this.productTransactionForm;
      data.type = "product-transaction-details";
      data.transaction_id = this.transaction_id;

      this.app.loading = true;
      this.updateProduct(data);
    },
    getId(url) {
      var regExp = /^.*(youtu.be\/|v\/|u\/\w\/|embed\/|watch\?v=|\&v=)([^#\&\?]*).*/;
      var match = url.match(regExp);

      if (match && match[2].length == 11) {
        return match[2];
      } else {
        return "error";
      }
    },
    /**
     * GET TRANSACTION DETAILS
     */
    getTransaction() {
      this.refreshButton = true;
      HTTP.get("api/transactions/getTransaction", {
        params: {
          transaction_id: this.$route.params.id
        }
      })
        .then(response => {
          if (response.data.Status) {
            this.transaction = response.data.transactionData;
            this.videoId =
              "https://www.youtube.com/embed/" +
              this.getId(this.transaction.url);
            this.refreshButton = false;
          }
        })
        .catch(error => {});
    },
    /**
     * openStepModle
     */
    openStep() {
      this.openStepDialog = true;
      this.$refs.addNewStepModel.editStep({ key: "add-step", param: "" });
    },
    /**
     * OPEN ADD USER DIALOG
     * ITS COMMANN DIALOG BOX FOR ADD AGENT ,ADD VENDOR
     */
    openAddUserDialogF(type) {
      console.log("Open add user" + type); //
      if (type == "add-agent") {
        this.openAddUserDialog = true;
        this.userModelTitle = "Add agent";
        this.commissionField = true;
        this.userCompanyField = false;
        this.modelType = "add-agent";
      } else if (type == "add-vendor") {
        console.log("add vendor");
        this.openAddUserDialog = true;
        this.userModelTitle = "Add vendor";
        this.commissionField = false;
        this.userCompanyField = true;
        this.modelType = "add-vendor";
      } else if (type == "add-step") {
        this.openAddUserDialog = true;
        this.userModelTitle = "Add step";
        this.modelType = "add-vendor";
      } else if (type == "add-transaction-user") {
        //TRANSACTIONAL USER
        this.openTUserDialog = true;
        this.modelType = "add-transaction-user";
        this.$refs.addTransactionUserModel.getAllTransactionUsers();
      }

      this.$refs.addUserModel.init(this.modelType);
    },

    //Close Dialog
    handleDialogClose(event) {
      console.log("handle dialog close" + event);
      if (event == "add-user") {
        console.log("add user");
        this.openAddUserDialog = false;
        this.$refs.addTransactionUserModel.init(this.transaction_id);
      } else if (event == "load-user") {
        console.log("load user");
        this.$refs.transactionAgents.getAllTransactionAgents();
        this.$refs.transactionVendors.getUsers();
      } else if (event == "transaction-steps") {
        //asssign steps to user
        console.log("handle assign step dialog here");
        this.openAddTransactionStepDialog = false;
        // this.openAddTransactionUserModal = false;
      } else if (event == "transaction-steps-open-transaction-users") {
        console.log("dsdf");
        this.openAddTransactionStepDialog = false;
      } else {
        console.log("Here is else part" + event);
        this.openAddUserDialog = false;
        this.openStepDialog = false;
        this.openTUserDialog = false;
        this.$refs.transactionSteps.getSteps();
      }
    },
    handleDialogOpen(event) {
      console.log("handle dialog" + event);
      if (event.key == "edit-vendor") {
        this.$refs.addUserModel.editVendor(event);
      } else if (event.key == "edit-step") {
        console.log("edit setp" + event);
        this.$refs.addNewStepModel.editStep(event);
        this.openStepDialog = true;
      } else if (event == "add-user") {
        console.log("adduser");
        this.openAddUserDialog = true;
        this.userModelTitle = "Add user";
        this.$refs.addUserModel.init("add-user");
        this.commissionField = false; //we dont need this field for Transactional user
      } else if (event == "add-steps") {
        console.log("add step close");
        // this.openStepDialog = false;
        this.openAddTransactionStepDialog = true;
        this.$refs.addTransactionStepModal.init(true);
        // this.openAddUserDialog = true;
      } else if (event == "steps-assigned-success") {
        console.log("on success close both");
        this.openAddTransactionStepDialog = false;
        this.openTUserDialog = false;
      } else if (event == "add-new-step") {
        console.log("add new step");
        //this.openAddTransactionStepDialog = true;
        this.openTUserDialog = false;  //close transactionuser popup
        this.$refs.addNewStepModel.init(true);
        this.openStepDialog = true; //it open a add new step
      }
    },
    formatDate(date) {
      var d = new Date(date),
        month = "" + (d.getMonth() + 1),
        day = "" + d.getDate(),
        year = d.getFullYear();

      if (month.length < 2) month = "0" + month;
      if (day.length < 2) day = "0" + day;
      console.log(year);
      console.log(month);
      console.log(day);

      return [year, month, day].join("-");
    }
  },
  filters: {
    formatDate: function(value) {
      let date = new Date(value);
      let monthNames = [
        "Jan",
        "Feb",
        "Mar",
        "Apr",
        "May",
        "Jun",
        "Jul",
        "Aug",
        "Sep",
        "Oct",
        "Nov",
        "Dec"
      ];

      let day = date.getDate();
      let monthIndex = date.getMonth();
      let year = date.getFullYear();

      return monthNames[monthIndex] + " " + day + ", " + year;
    }
  },
  validations: {
    productForm: {
      description: {
        required
      }
    },
    productTransactionForm: {
      sales_price: {
        required
      },
      additional_fee: {
        required
      },
      execuate_date: {
        required
      },
      closed_date: {
        required
      }
    },
    userDetailsForm: {
      first_name: {
        required
      },
      last_name: {
        required
      },
      street_no: {
        required
      },
      street_address: {
        required
      },
      city: {
        required
      },
      state: {
        required
      }
    }
  },
  computed: {
    fullName: function() {
      var firstName = this.transaction.first_name
        ? this.transaction.first_name
        : "";
      var lastName = this.transaction.last_name
        ? this.transaction.last_name
        : "";
      const fullName = firstName + " " + lastName;
      return fullName;
    },
    streetAddress: function() {
      var street_no = this.transaction.street_no
        ? this.transaction.street_no
        : "";
      var street_addr = this.transaction.street_address
        ? this.transaction.street_address
        : "";

      const streetAddress = street_no + " " + street_addr;
      return streetAddress;
    },
    CSAddress: function() {
      var street_no = this.transaction.street_no
        ? this.transaction.street_no
        : "";
      var street_addr = this.transaction.street_address
        ? this.transaction.street_address
        : "";

      const streetAddress = street_no + " " + street_addr;
      return streetAddress;
    }
  },
  watch: {}
};
</script>

<style scoped>
</style>
