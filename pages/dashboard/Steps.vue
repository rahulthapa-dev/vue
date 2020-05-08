<template>
  <div class="content">
    <!-- <md-dialog :md-active.sync="dialogActive" class="modal-small">
        <md-dialog-title class="modal-title d-block py-3 px-4 bg-light position-relative">
            <h4 class="my-0 md-headline text-left text-primary">Confirm</h4>
            <md-button @click="dialogActive = false" class="md-icon-button position-absolute">
                <md-icon class="text-primary">close</md-icon>
            </md-button>
        </md-dialog-title>
        <div class="md-dialog-content p-0">
            <md-content class="px-4">
                <div class="md-layout">
                    <div class="md-layout-item md-size-100 py-2">
                        <h4 class="text-center sub-title">Are you sure want to delete?</h4>
                    </div>
                </div>
            </md-content>
        </div>
        <md-dialog-actions class="flex-wrap justify-content-center mb-3">
            <div class="d-block w-100 text-center">
                <md-button type="submit" class="md-primary button-primary-custom w-100">
                    <span>Agree</span>
                </md-button>
                <br />
                <md-button :md-ripple="false" @click="dialogActive = false" class="cancel-link lighter-description text-capitalize">Disagree</md-button>
            </div>
        </md-dialog-actions>
    </md-dialog>-->
    <md-dialog-confirm
      :md-active.sync="dialogActive"
      md-title="Confirmation?"
      md-content="Are you sure want to delete?."
      md-confirm-text="Agree"
      md-cancel-text="Disagree"
      @md-cancel="onCancel"
      @md-confirm="onConfirm"
    />
    <md-dialog :md-active.sync="showStep">
      <md-dialog-title class="modal-title d-block py-3 px-4 bg-light position-relative">
        <h4 class="my-0 md-headline text-left text-primary">Add Step</h4>
        <md-button @click="showStep = false" class="md-icon-button position-absolute">
          <md-icon class="text-primary">close</md-icon>
        </md-button>
      </md-dialog-title>
      <div class="md-dialog-content p-0">
        <form @submit.prevent="addSteps">
          <md-content md-dynamic-height class="px-4">
            <div class="md-layout">
              <div class="md-layout-item md-small-size-100 md-size-50">
                <md-field slot="inputs" :class="app.getValidationClass($v,'step_title')">
                  <label>Step Title</label>
                  <md-input v-model="step_title"></md-input>
                  <span v-if="!$v.step_title.required" class="md-error">Title is required</span>
                </md-field>
              </div>
              <div class="md-layout-item md-small-size-100 md-size-50">
                <md-field slot="inputs" :class="app.getValidationClass($v,'url')">
                  <label>URL (Vimeo, Youtube, ...)</label>
                  <md-input v-model="url"></md-input>
                  <span v-if="!$v.url.urlValid" class="md-error">Url invalid</span>
                </md-field>
              </div>
              <div class="md-layout-item md-small-size-100 md-size-100">
                <md-field slot="inputs" :class="app.getValidationClass($v,'description')">
                  <label>Description</label>
                  <md-textarea
                    class="px-0"
                    v-model="description"
                    value="At vero eos et accusamus et iusto odio dignissimos ducimus qui blanditiis praesentium voluptatum deleniti atque corrupti quos dolores et quas molestias."
                  ></md-textarea>
                  <span v-if="!$v.description.required" class="md-error">Description is required</span>
                  <span v-if="!$v.description.maxLength" class="md-error">maxLength is 150</span>
                </md-field>
              </div>
            </div>
          </md-content>
          <md-dialog-actions class="flex-wrap justify-content-center">
            <div class="d-block w-100 text-center">
              <md-button type="submit" class="md-primary w-100 button-primary-custom mb-2 mt-3">
                <LoadingButtonLoader
                  :typeEnable="app.checkType('socialLoader')"
                  v-bind:enable="app.loading"
                  class="mr-1"
                />
                <span v-if="!editStatus">Add</span>
                <span v-else>Update</span>
              </md-button>
              <br />
              <md-button
                :md-ripple="false"
                @click="showStep = false"
                class="cancel-link lighter-description mt-2 text-capitalize mb-2"
              >Cancel</md-button>
            </div>
          </md-dialog-actions>
        </form>
      </div>
    </md-dialog>
    <md-dialog :md-active.sync="showActiveStep">
      <!--Active Steps Dialog--->
      <md-dialog-title class="modal-title d-block py-3 px-4 bg-light position-relative">
        <h4 class="my-0 md-headline text-left text-primary">Active Step</h4>
        <md-button @click="showActiveStep = false" class="md-icon-button position-absolute">
          <md-icon class="text-primary">close</md-icon>
        </md-button>
      </md-dialog-title>
      <div class="md-dialog-content p-0">
        <md-content md-dynamic-height class="px-4">
          <md-table class="mb-3 system-steps" md-sort="name" md-sort-order="asc">
            <md-table-row class="table-head">
              <md-table-head>Steps Title</md-table-head>
              <md-table-head>Default Description</md-table-head>
              <md-table-head>Default Videos</md-table-head>
              <md-table-head>Created</md-table-head>
              <md-table-head></md-table-head>
            </md-table-row>

            <md-table-row slot="md-table-row" v-for="(item) in activeItems" :key="item.id">
              <md-table-cell md-label="Steps Title" class="steps">{{ item.steps_title }}</md-table-cell>
              <md-table-cell md-label="Default Description">{{ item.description }}</md-table-cell>
              <md-table-cell md-label="Default Videos">
                <a href class="brand-primary">{{ item.video }}</a>
              </md-table-cell>
              <md-table-cell md-label="Created">{{ item.created_at|formatDate }}</md-table-cell>
              <md-table-cell md-label class="justify-content-end">
                <md-menu md-direction="bottom-end">
                  <md-button md-menu-trigger class="md-icon-button icon-button-flat">
                    <md-icon>more_vert</md-icon>
                  </md-button>
                  <md-menu-content class="menu-content-custom">
                    <md-menu-item @click="editStep(item.id)">Edit</md-menu-item>
                    <md-menu-item @click="archiveStep(item.id)">Archive</md-menu-item>
                  </md-menu-content>
                </md-menu>
              </md-table-cell>
            </md-table-row>
          </md-table>
          <div class="md-layout">
            <div
              class="md-layout-item md-medium-size-50 md-xsmall-size-100 md-size-50 d-flex align-center"
            >
              <p
                class="description font-weight-medium"
                v-if="activeSteps"
              >Displaying {{ activeItems.length }} of {{ activeSteps.length }} records</p>
            </div>
            <div class="md-layout-item md-medium-size-50 md-xsmall-size-100 md-size-50">
              <!-- for pagination you can check vue material kit -->
              <ul class="pagination">
                <li
                  class="page-item prev-page"
                  v-if="page != 1"
                  v-bind:class="[page == pageNumber ? 'active' : '']"
                >
                  <a @click="page--" class="page-link" aria-label="Previous">
                    <i class="fas fa-angle-double-left"></i>
                    <span v-if="withText" class="pr-1">Prev</span>
                  </a>
                </li>
                <!-- {{pages.slice(page-1, page+5)}} -->
                <li v-if="activeSteps.length > 5">
                  <ul class="pagination">
                    <li
                      class="page-item"
                      v-for="pageNumber in pages"
                      v-bind:key="pageNumber"
                      v-bind:class="[page == pageNumber ? 'active' : '']"
                    >
                      <a @click="page = pageNumber" class="page-link">{{ pageNumber }}</a>
                    </li>
                  </ul>
                </li>

                <li
                  class="page-item page-pre next-page"
                  v-if="page < pages.length"
                  v-bind:class="[page == pageNumber ? 'active' : '']"
                >
                  <a @click="page++" class="page-link" aria-label="Next">
                    <span class="pl-1">Next</span>
                    <i class="fas fa-angle-double-right"></i>
                  </a>
                </li>
              </ul>
            </div>
          </div>
        </md-content>
      </div>
    </md-dialog>
    <md-dialog :md-active.sync="showActiveSystemStep">
      <!--System Steps  Dialog-->
      <md-dialog-title class="modal-title d-block py-3 px-4 bg-light position-relative">
        <h4 class="my-0 md-headline text-left text-primary">Active Company Step</h4>
        <md-button @click="showActiveSystemStep = false" class="md-icon-button position-absolute">
          <md-icon class="text-primary">close</md-icon>
        </md-button>
      </md-dialog-title>
      <div class="md-dialog-content p-0">
        <md-content md-dynamic-height class="px-4">
          <md-table class="mb-3 system-steps" md-sort="name" md-sort-order="asc">
            <md-table-row class="table-head">
              <md-table-head>Steps Title</md-table-head>
              <md-table-head>Default Description</md-table-head>
              <md-table-head>Default Videos</md-table-head>
              <md-table-head>Created</md-table-head>
              <md-table-head></md-table-head>
            </md-table-row>

            <md-table-row slot="md-table-row" v-for="(item) in systemItems" :key="item.id">
              <md-table-cell md-label="Steps Title" class="steps">{{ item.steps_title }}</md-table-cell>
              <md-table-cell md-label="Default Description">{{ item.description }}</md-table-cell>
              <md-table-cell md-label="Default Videos">
                <a href class="brand-primary">{{ item.video }}</a>
              </md-table-cell>
              <md-table-cell md-label="Created">{{ item.created_at|formatDate }}</md-table-cell>
              <md-table-cell md-label class="justify-content-end">
                <md-menu md-direction="bottom-end">
                  <md-button md-menu-trigger class="md-icon-button icon-button-flat">
                    <md-icon>more_vert</md-icon>
                  </md-button>
                  <md-menu-content class="menu-content-custom">
                    <md-menu-item @click="editStep(item.id)">Edit</md-menu-item>
                    <md-menu-item @click="archiveStep(item.id)">Archive</md-menu-item>
                  </md-menu-content>
                </md-menu>
              </md-table-cell>
            </md-table-row>
          </md-table>
          <div class="md-layout">
            <div
              class="md-layout-item md-medium-size-50 md-xsmall-size-100 md-size-50 d-flex align-center"
            >
              <p
                class="description font-weight-medium"
                v-if="systemSteps"
              >Displaying {{ systemItems.length }} of {{ systemSteps.length }} records</p>
            </div>
            <div class="md-layout-item md-medium-size-50 md-xsmall-size-100 md-size-50">
              <!-- for pagination you can check vue material kit -->
              <ul class="pagination">
                <li
                  class="page-item prev-page"
                  v-if="page2 != 1"
                  v-bind:class="[page2 == pageNumber2 ? 'active' : '']"
                >
                  <a @click="page2--" class="page-link" aria-label="Previous">
                    <i class="fas fa-angle-double-left"></i>
                    <span v-if="withText" class="pr-1">Prev</span>
                  </a>
                </li>
                <!--pages2.slice(page2-1, page2+5)--->
                <li v-if="systemSteps.length > 5">
                  <ul class="pagination">
                    <li
                      class="page-item"
                      v-for="pageNumber2 in pages2"
                      v-bind:key="pageNumber2"
                      v-bind:class="[page2 == pageNumber2 ? 'active' : '']"
                    >
                      <a @click="page2 = pageNumber2" class="page-link">{{ pageNumber2 }}</a>
                    </li>
                  </ul>
                </li>

                <li
                  class="page-item page-pre next-page"
                  v-if="page2 < pages2.length"
                  v-bind:class="[page2 == pageNumber2 ? 'active' : '']"
                >
                  <a @click="page2++" class="page-link" aria-label="Next">
                    <span class="pl-1">Next</span>
                    <i class="fas fa-angle-double-right"></i>
                  </a>
                </li>
              </ul>
            </div>
          </div>
        </md-content>
      </div>
    </md-dialog>
    <md-dialog :md-active.sync="showArchiveStep">
      <!--Archive  Steps Dialog-->
      <md-dialog-title class="modal-title d-block py-3 px-4 bg-light position-relative">
        <h4 class="my-0 md-headline text-left text-primary">Archive Step</h4>
        <md-button @click="showArchiveStep = false" class="md-icon-button position-absolute">
          <md-icon class="text-primary">close</md-icon>
        </md-button>
      </md-dialog-title>
      <div class="md-dialog-content p-0">
        <md-content md-dynamic-height class="px-4">
          <md-table class="mb-3 system-steps" md-sort="name" md-sort-order="asc">
            <md-table-row>
              <md-table-head>Steps Title</md-table-head>
              <md-table-head>Default Description</md-table-head>
              <md-table-head>Default Videos</md-table-head>
              <md-table-head>Created</md-table-head>
            </md-table-row>

            <md-table-row slot="md-table-row" v-for="(item) in archiveItems" :key="item.id">
              <md-table-cell md-label="Steps Title" class="steps">{{ item.steps_title }}</md-table-cell>
              <md-table-cell md-label="Default Description">{{ item.description }}</md-table-cell>
              <md-table-cell md-label="Default Videos">
                <a href class="brand-primary">{{ item.video }}</a>
              </md-table-cell>
              <md-table-cell md-label="Created">{{ item.created_at|formatDate }}</md-table-cell>
              <!-- <md-table-cell md-label class="justify-content-end">
                            <md-menu md-direction="bottom-end">
                                <md-button md-menu-trigger class="md-icon-button icon-button-flat">
                                    <md-icon>more_vert</md-icon>
                                </md-button>
                                <md-menu-content class="menu-content-custom">
                                    <md-menu-item @click="editStep(item.id)">Edit</md-menu-item>
                                    <md-menu-item @click="archiveStep(item.id)">Delete</md-menu-item>
                                </md-menu-content>
                            </md-menu>
              </md-table-cell>-->
            </md-table-row>
          </md-table>
          <div class="md-layout">
            <div
              class="md-layout-item md-medium-size-50 md-xsmall-size-100 md-size-50 d-flex align-center"
            >
              <p
                class="description font-weight-medium"
                v-if="archiveSteps"
              >Displaying {{ archiveItems.length }} of {{ archiveSteps.length }} records</p>
            </div>
            <div class="md-layout-item md-medium-size-50 md-xsmall-size-100 md-size-50">
              <!-- for pagination you can check vue material kit -->
              <ul class="pagination">
                <li
                  class="page-item prev-page"
                  v-if="page3 != 1"
                  v-bind:class="[page3 == pageNumber3 ? 'active' : '']"
                >
                  <a @click="page3--" class="page-link" aria-label="Previous">
                    <i class="fas fa-angle-double-left"></i>
                    <span v-if="withText" class="pr-1">Prev</span>
                  </a>
                </li>
                <!--pages3.slice(page3-1, page2+5)-->
                <li v-if="archiveSteps.length > 5">
                  <ul class="pagination">
                    <li
                      class="page-item"
                      v-for="pageNumber3 in pages3"
                      v-bind:key="pageNumber3"
                      v-bind:class="[page3 == pageNumber3 ? 'active' : '']"
                    >
                      <a @click="page3 = pageNumber3" class="page-link">{{ pageNumber3 }}</a>
                    </li>
                  </ul>
                </li>

                <li
                  class="page-item page-pre next-page"
                  v-if="page3 < pages3.length"
                  v-bind:class="[page3 == pageNumber3 ? 'active' : '']"
                >
                  <a @click="page3++" class="page-link" aria-label="Next">
                    <span class="pl-1">Next</span>
                    <i class="fas fa-angle-double-right"></i>
                  </a>
                </li>
              </ul>
            </div>
          </div>
        </md-content>
      </div>
    </md-dialog>
    <div class="md-layout flex-md-column-reverse">
      <div class="md-layout-item md-medium-size-100 md-xsmall-size-100 md-size-100">
        <md-card class="md-card-plain md-card-custom custom-shadow radius-10 mt-0">
          <md-card-header class="px-4 mb-0 mt-0">
            <div class="md-headline">
              <span class="text-primary">Steps Settings</span>
              <span class="brand-primary">
                <md-icon class="brand-primary">keyboard_arrow_right</md-icon>Steps
              </span>
            </div>
          </md-card-header>
          <!--Active steps-->
          <md-card-content class="px-4 py-3">
            <div class="md-layout">
              <div class="md-layout-item d-flex justify-content-between mb-3">
                <div class="d-flex align-center justify-start">
                  <label
                    for
                    class="mr-3 active-label description d-flex align-center justify-content-center"
                  >Active</label>
                  <h3 class="text-primary font-20 my-0">Active Steps</h3>
                </div>
                <md-button class="md-primary md-icon-button" @click="openSteps(1)">
                  <md-icon>add</md-icon>
                </md-button>
              </div>
            </div>

            <md-table class="mb-3 active-steps" md-sort="name" md-sort-order="asc">
              <md-table-row class="table-head">
                <md-table-head>Steps Title</md-table-head>
                <md-table-head>Default Description</md-table-head>
                <md-table-head>Default Videos</md-table-head>
                <md-table-head>Created on</md-table-head>
                <md-table-head></md-table-head>
              </md-table-row>
              <md-table-row
                slot="md-table-row"
                v-for="(item) in activeSteps.slice(0,2)"
                :key="item.id"
              >
                <md-table-cell md-label="Steps Title" class="steps">{{ item.steps_title }}</md-table-cell>
                <md-table-cell md-label="Default Description">{{ item.description }}</md-table-cell>
                <md-table-cell md-label="Default Videos">
                  <a href class="brand-primary">{{ item.video }}</a>
                </md-table-cell>
                <md-table-cell md-label="Created">{{ item.created_at|formatDate }}</md-table-cell>
                <md-table-cell md-label class="justify-content-end">
                  <md-menu md-direction="bottom-end">
                    <md-button md-menu-trigger class="md-icon-button icon-button-flat">
                      <md-icon>more_vert</md-icon>
                    </md-button>
                    <md-menu-content class="menu-content-custom">
                      <md-menu-item @click="editStep(item.id)">Edit</md-menu-item>
                      <md-menu-item @click="archiveStep(item.id)">Archive</md-menu-item>
                    </md-menu-content>
                  </md-menu>
                </md-table-cell>
              </md-table-row>
            </md-table>
            <div class="md-layout">
              <div
                class="md-layout-item md-medium-size-50 md-xsmall-size-100 md-size-50 d-flex align-center"
              >
                <!-- <p class="description font-weight-medium">Displaying 10 of 20 records</p> -->

                <p
                  class="description font-weight-medium"
                  v-if="activeSteps.length > 1"
                >Displaying 2 of {{ activeSteps.length }} records</p>
              </div>
              <div
                class="md-layout-item md-medium-size-50 md-xsmall-size-100 md-size-50 justify-content-end d-flex"
              >
                <md-button @click="showActiveStep = true" class="load-more-button md-primary mr-0">
                  <i class="fas fa-sync-alt"></i> View all
                </md-button>
              </div>
            </div>
          </md-card-content>
          <!--Active System steps-->
          <md-card-content class="px-4 py-3">
            <div class="md-layout">
              <div class="md-layout-item d-flex justify-content-between mb-3">
                <div class="d-flex align-center justify-start">
                  <label
                    for
                    class="mr-3 active-label description d-flex align-center justify-content-center"
                  >Active</label>
                  <h3 class="text-primary font-20 my-0">Active Company Steps</h3>
                </div>
                <md-button class="md-primary md-icon-button" @click="openSteps(2)">
                  <md-icon>add</md-icon>
                </md-button>
              </div>
            </div>

            <md-table class="mb-3 system-steps" md-sort="name" md-sort-order="asc">
              <md-table-row class="table-head">
                <md-table-head>Steps Title</md-table-head>
                <md-table-head>Default Description</md-table-head>
                <md-table-head>Default Videos</md-table-head>
                <md-table-head>Created on</md-table-head>
                <md-table-head></md-table-head>
              </md-table-row>

              <md-table-row
                slot="md-table-row"
                v-for="(item) in systemSteps.slice(0,2)"
                :key="item.id"
              >
                <md-table-cell md-label="Steps Title" class="steps">{{ item.steps_title }}</md-table-cell>
                <md-table-cell md-label="Default Description">{{ item.description }}</md-table-cell>
                <md-table-cell md-label="Default Videos">
                  <a href class="brand-primary">{{ item.video }}</a>
                </md-table-cell>
                <md-table-cell md-label="Created">{{ item.created_at|formatDate }}</md-table-cell>
                <md-table-cell md-label class="justify-content-end">
                  <md-menu md-direction="bottom-end">
                    <md-button md-menu-trigger class="md-icon-button icon-button-flat">
                      <md-icon>more_vert</md-icon>
                    </md-button>
                    <md-menu-content class="menu-content-custom">
                      <md-menu-item @click="editStep(item.id)">Edit</md-menu-item>
                      <md-menu-item @click="archiveStep(item.id)">Archive</md-menu-item>
                    </md-menu-content>
                  </md-menu>
                </md-table-cell>
              </md-table-row>
            </md-table>
            <div class="md-layout">
              <div
                class="md-layout-item md-medium-size-50 md-xsmall-size-100 md-size-50 d-flex align-center"
              >
                <p
                  class="description font-weight-medium"
                  v-if="systemSteps.length > 1"
                >Displaying 2 of {{ systemSteps.length }} records</p>
              </div>
              <div
                class="md-layout-item md-medium-size-50 md-xsmall-size-100 md-size-50 justify-content-end d-flex"
              >
                <md-button
                  @click="showActiveSystemStep = true"
                  class="load-more-button md-primary mr-0"
                >
                  <i class="fas fa-sync-alt"></i> View all
                </md-button>
              </div>
            </div>
          </md-card-content>
          <!--Archived steps-->
          <md-card-content class="px-4 py-3">
            <div class="md-layout">
              <div class="md-layout-item d-flex justify-content-between mb-3">
                <div class="d-flex align-center justify-start">
                  <label
                    for
                    class="mr-3 archived-label description d-flex align-center justify-content-center"
                  >Archived</label>
                  <h3 class="text-primary font-20 my-0">Archived Steps</h3>
                </div>
              </div>
            </div>

            <md-table class="mb-3 active-steps" md-sort="name" md-sort-order="asc">
              <md-table-row class="table-head">
                <md-table-head>Steps Title</md-table-head>
                <md-table-head>Default Description</md-table-head>
                <md-table-head>Default Videos</md-table-head>
                <md-table-head>Created on</md-table-head>
                <md-table-head></md-table-head>
              </md-table-row>
              <md-table-row
                slot="md-table-row"
                v-for="(item) in archiveSteps.slice(0,2)"
                :key="item.id"
              >
                <md-table-cell md-label="Steps Title" class="steps">{{ item.steps_title }}</md-table-cell>
                <md-table-cell md-label="Default Description">{{ item.description }}</md-table-cell>
                <md-table-cell md-label="Default Videos">
                  <a href class="brand-primary">{{ item.video }}</a>
                </md-table-cell>
                <md-table-cell md-label="Created">{{ item.created_at|formatDate }}</md-table-cell>
                <!-- <md-table-cell md-label class="justify-content-end">
                                <md-menu md-direction="bottom-end">
                                    <md-button md-menu-trigger class="md-icon-button icon-button-flat">
                                        <md-icon>more_vert</md-icon>
                                    </md-button>
                                    <md-menu-content class="menu-content-custom">
                                        <md-menu-item>Edit</md-menu-item>
                                        <md-menu-item @click="deleteStep(item.id)">Delete</md-menu-item>
                                    </md-menu-content>
                                </md-menu>
                </md-table-cell>-->
              </md-table-row>
            </md-table>
            <div class="md-layout">
              <div
                class="md-layout-item md-medium-size-50 md-xsmall-size-100 md-size-50 d-flex align-center"
              >
                <p
                  class="description font-weight-medium"
                  v-if="archiveSteps.length > 2"
                >Displaying 2 of {{ archiveSteps.length }} records</p>
              </div>
              <div
                class="md-layout-item md-medium-size-50 md-xsmall-size-100 md-size-50 justify-content-end d-flex"
              >
                <md-button @click="showArchiveStep = true" class="load-more-button md-primary mr-0">
                  <i class="fas fa-sync-alt"></i> View all
                </md-button>
              </div>
            </div>
          </md-card-content>
        </md-card>
      </div>
    </div>
  </div>
</template>

<script>
import { HTTP } from "../../httpCommon";
import LoadingBar from "../../components/LoadingBar";
import LoadingButtonLoader from "../../components/LoadingButtonLoader";
import {
  required,
  email,
  minLength,
  maxLength,
  sameAs
} from "vuelidate/lib/validators";

const isUrl = value =>
  /^(|https?:\/\/[\w\-_]+(\.[\w\-_]+)+([\w\-\.,@?^=%&amp;:/~\+#]*[\w\-\@?^=%&amp;/~\+#])?)$/.test(
    value
  );

export default {
  name: "Steps",
  props: ["app"],
  components: {
    LoadingBar,
    LoadingButtonLoader
  },
  data: function() {
    return {
      showStep: false,
      showActiveStep: false,
      showActiveSystemStep: false,
      showArchiveStep: false,
      activeSteps: [
        {
          id: "1",
          steps_title: "Hunger",
          description: "This is a sample description",
          video: "https://vimeo.com/123",
          created_at: "Aug 30, 2019"
        }
      ],
      systemSteps: [
        {
          id: "1",
          steps_title: "Hunger",
          description: "This is a sample description",
          video: "https://vimeo.com/123",
          created_at: "Aug 30, 2019"
        },
        {
          id: "2",
          steps_title: "Moee",
          description: "This is a sample description",
          video: "https://vimeo.com/123",
          created_at: "Aug 30, 2019"
        }
      ],
      archiveSteps: [
        {
          id: "1",
          steps_title: "Hunger",
          description: "This is a sample description",
          video: "https://vimeo.com/123",
          created_at: "Aug 30, 2019"
        },
        {
          id: "2",
          steps_title: "Moee",
          description: "This is a sample description",
          video: "https://vimeo.com/123",
          created_at: "Aug 30, 2019"
        }
      ],
      totalActiveSteps: "",
      totalSystemSteps: "",
      totalArchiveSteps: "",
      step_title: "",
      url: "",
      description: "",
      step_id: "",
      items: "",
      totalItems: "",
      deleteStepId: "",
      page: 1, //current page
      perPage: 5,
      pages: [],
      pageNumber: "",
      page2: 1, //current page
      perPage2: 5,
      pages2: [],
      pageNumber2: "",
      page3: 1, //current page
      perPage3: 5,
      pages3: [],
      pageNumber3: "",
      withText: Boolean,
      dialogActive: false,
      editStatus: false,
      steps_type: ""
    };
  },
  validations: {
    step_title: {
      required
    },
    url: {
      urlValid: isUrl
    },
    description: {
      required,
      maxLength: maxLength(300)
    }
  },
  mounted() {
    this.init();
  },
  computed: {
    activeItems() {
      console.log("active item change");
      return this.paginate(this.activeSteps);
    },
    systemItems() {
      console.log("system item change");
      return this.paginate2(this.systemSteps);
    },
    archiveItems() {
      console.log("archive item change");
      return this.paginate3(this.archiveSteps);
    }
  },
  watch: {},
  methods: {
    init(fetch = null) {
      HTTP.get("api/steps/getSteps")
        .then(response => {
          if (response.data.Status) {
            this.activeSteps = response.data.ActiveSteps;
            // console.log(this.activeSteps);
            this.systemSteps = response.data.SystemSteps;
            this.archiveSteps = response.data.ArchiveSteps;

            this.setPages(this.activeSteps.length);
            this.setPages2(this.systemSteps.length);
            this.setPages3(this.archiveSteps.length);
          }
        })
        .catch(error => {});
    },
    openSteps(option) {
      this.editStatus = false;
      this.showStep = true;
      this.steps_type = option;
      this.step_title = "";
      this.url = "";
      this.description = "";
    },
    addSteps() {
      this.$v.$touch();
      if (this.$v.$error) return;
      this.app.loading = true;
      // this.app.loaderType = this.app.checkType("simpleLoader");
      const data = {
        step_id: this.step_id,
        step_title: this.step_title,
        url: this.url,
        description: this.description,
        steps_type: this.steps_type
      };

      // console.log(data);

      if (this.editStatus) {
        HTTP.put("api/steps/updateSteps", data)
          .then(response => {
            if (response.data.Status) {
              this.app.loading = false;
              this.init();
              this.$toasted
                .show(response.data.Message, {
                  type: "success"
                  // icon: "error_outline"
                })
                .goAway(1500);

              this.showStep = false;
            }
          })
          .catch(error => {
            this.app.loading = false;
          });
      } else {
        HTTP.post("api/steps/addSteps", data)
          .then(response => {
            if (response.data.Status) {
              this.app.loading = false;
              this.init();
              this.$toasted
                .show(response.data.Message, {
                  type: "success"
                  // icon: "error_outline"
                })
                .goAway(1500);
              this.showStep = false;
            }
          })
          .catch(error => {
            this.app.loading = false;
          });
      }
    },
    setPages(totalItems) {
      this.pages = [];
      let numberOfPages = Math.ceil(totalItems / this.perPage);
      for (let index = 1; index <= numberOfPages; index++) {
        this.pages.push(index);
      }
      // debugger;
    },
    setPages2(totalItems) {
      this.pages2 = [];
      let numberOfPages2 = Math.ceil(totalItems / this.perPage);
      for (let index = 1; index <= numberOfPages2; index++) {
        this.pages2.push(index);
      }
      // debugger;
    },
    setPages3(totalItems) {
      this.pages3 = [];
      let numberOfPages = Math.ceil(totalItems / this.perPage);
      for (let index = 1; index <= numberOfPages; index++) {
        this.pages3.push(index);
      }
      // debugger;
    },
    paginate(items) {
      let page = this.page;
      let perPage = this.perPage;
      let from = page * perPage - perPage;
      let to = page * perPage;
      return items.slice(from, to);
    },
    paginate2(items) {
      let page = this.page2;
      let perPage = this.perPage2;
      let from = page * perPage - perPage;
      let to = page * perPage;
      return items.slice(from, to);
    },
    paginate3(items) {
      let page = this.page3;
      let perPage = this.perPage3;
      let from = page * perPage - perPage;
      let to = page * perPage;
      return items.slice(from, to);
    },
    archiveStep(id) {
      this.app.loading = true;
      const data = {
        step_id: id
      };
      HTTP.post("api/steps/archiveSteps", data)
        .then(response => {
          if (response.data.Status) {
            this.app.loading = false;
            this.init(); //refresh new results
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
    editStep(id) {
      console.log(id);
      this.showActiveStep = false;
      this.showActiveSystemStep = false;
      // this.$v.$reset();
      this.editStatus = true;
      let checkIndex1 = this.activeSteps
        .map((item, index) => {
          return item.id;
        })
        .indexOf(id);
      let checkIndex2 = this.systemSteps
        .map((item, index) => {
          return item.id;
        })
        .indexOf(id);

      if (checkIndex1 !== -1) {
        let step = this.activeSteps[checkIndex1];
        this.step_id = step.id;
        this.step_title = step.steps_title;

        if (step.video) {
          this.url = step.video;
        }
        this.description = step.description;
      }
      if (checkIndex2 !== -1) {
        let step = this.systemSteps[checkIndex2];
        this.step_id = step.id;
        this.step_title = step.steps_title;
        if (step.video) {
          this.url = step.video;
        }
        this.description = step.description;
      }
      this.showStep = true;
      // debugger;
    },
    deleteStep(id) {
      this.dialogActive = true;
      this.deleteStepId = id;
    },
    onConfirm() {
      this.app.loading = true;

      if (this.deleteStepId) {
        HTTP.get("api/steps/deleteSteps", {
          params: {
            deleteStepId: this.deleteStepId
          }
        })
          .then(response => {
            if (response.data.Status) {
              this.app.loading = false;
              this.init(); //refresh new results
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
      }
    },
    onCancel() {}
  }
};
</script>

<style lang="scss" scoped>
.system-steps {
  th,
  td {
    &:nth-child(1) {
      max-width: 109px;
      min-width: 109px;
      word-break: break-all;
      white-space: normal;
      width: 15%;
    }

    &:nth-child(2) {
      width: 25%;
      word-break: break-word;
      white-space: normal;
      min-width: 227px;
    }

    &:nth-child(3) {
      width: 25%;
      flex-wrap: wrap;
      word-break: break-word;
      white-space: normal;
      min-width: 227px;
    }

    &:nth-child(4) {
      width: 15%;
      flex-wrap: wrap;
    }

    &:nth-child(5) {
      width: 5%;
    }
  }
}

.active-steps {
  th,
  td {
    &:nth-child(1) {
      max-width: 109px;
      min-width: 109px;
      width: 15%;
      word-break: break-all;
      white-space: normal;
    }

    &:nth-child(2) {
      width: 25%;
      word-break: break-word;
      white-space: normal;
      min-width: 227px;
    }

    &:nth-child(3) {
      width: 25%;
      word-break: break-word;
      white-space: normal;
      min-width: 227px;
    }

    &:nth-child(4) {
      width: 15%;
    }

    &:nth-child(5) {
      width: 5%;
    }
  }
}
</style>
