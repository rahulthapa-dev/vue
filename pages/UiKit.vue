<template>
  <div class="wrapper ui-kit">
    <div class="container" style="flex-wrap:wrap;">
      <section class="d-block mb-3 w-100">
        <h1>Typography</h1>
        <h4 slot="title" class="card-title text-primary">card-title 30px</h4>
        <h4 class="md-headline text-left text-primary">md-headline 24px</h4>
        <h4 class="md-title text-left text-primary">md-title 18px</h4>
        <p class="description sub-title">description sub-title 18px</p>
        <p class="description description-confirm">description description-confirm 18px</p>
        <p class="description">description 16px</p>
        <p
          class="description-normal text-description text-left font-weight-regular"
        >description-normal text-description 14px</p>
        <h3 class="font-weight-regular">Regular font weight font-weight-regular</h3>
        <h3 class="font-weight-medium">Medium font weight font-weight-medium</h3>
        <h3 class="font-weight-semi-bold">Semi Bold font weight font-weight-semi-bold</h3>
      </section>
      <section class="d-block mb-3 w-100">
        <h1 class="mb-4">Layout</h1>
        <div class="px-3 layout-1 mb-3">
          <div class="md-layout">
            <div class="md-layout-item md-medium-size-33 md-small-size-50 md-xsmall-size-100"></div>
            <div class="md-layout-item md-medium-size-33 md-small-size-50 md-xsmall-size-100"></div>
            <div class="md-layout-item md-medium-size-33 md-small-size-50 md-xsmall-size-100"></div>
          </div>
        </div>
        <div class="px-3 layout-2 mb-3">
          <div class="md-layout md-gutter">
            <div class="md-layout-item"></div>
            <div class="md-layout-item"></div>
            <div class="md-layout-item"></div>
          </div>
        </div>
        <div class="layout-3 mb-3">
          <div class="md-layout md-gutter">
            <div class="md-layout-item md-layout md-gutter">
              <div class="md-layout-item"></div>
              <div class="md-layout-item"></div>
              <div class="md-layout-item"></div>
            </div>

            <div class="md-layout-item md-layout md-gutter">
              <div class="md-layout-item"></div>
              <div class="md-layout-item"></div>
            </div>

            <div class="md-layout-item md-layout md-gutter">
              <div class="md-layout-item"></div>
              <div class="md-layout-item"></div>
              <div class="md-layout-item"></div>
              <div class="md-layout-item"></div>
            </div>
          </div>
        </div>
      </section>
      <section class="d-block mb-3 w-100">
        <h1>Tab</h1>
        <div class="modal-content p-4">
          <md-tabs class="md-success" md-alignment="centered">
            <md-tab id="tab-home" md-label="Monthly">
              <div class="md-layout md-gutter">
                <div
                  class="md-layout-item md-medium-size-33 md-small-size-50 md-xsmall-size-100 mb-3"
                >
                  <div class="md-card-custom border-box pricing-box">
                    <div class="pricing-head-content py-4 px-3">
                      <span class="d-block text-uppercase pb-1">Tier One</span>
                      <h3 class="md-headline d-block text-primary pb-1 my-0">$ 100</h3>
                      <p class="md-body-1">Best for individual agents</p>
                    </div>
                    <div class="pricing-body-content py-3 px-3">
                      <md-list class="md-listing">
                        <md-list-item class="position-relative">
                          <md-icon
                            class="md-icon md-icon-font md-theme-default position-absolute"
                          >done</md-icon>
                          <span class="md-list-item-text">Item One</span>
                        </md-list-item>
                        <md-list-item class="position-relative">
                          <md-icon
                            class="md-icon md-icon-font md-theme-default position-absolute"
                          >done</md-icon>
                          <span class="md-list-item-text">Item Two</span>
                        </md-list-item>
                        <md-list-item class="position-relative">
                          <md-icon
                            class="md-icon md-icon-font md-theme-default position-absolute text-disabled"
                          >close</md-icon>
                          <span class="md-list-item-text text-disabled">Item Three</span>
                        </md-list-item>
                      </md-list>
                      <button
                        class="md-button md-primary md-theme-default button-custom-regular button-outline"
                      >Buy now</button>
                    </div>
                  </div>
                </div>
                <div
                  class="md-layout-item md-medium-size-33 md-small-size-50 md-xsmall-size-100 mb-3"
                >
                  <div class="md-card-custom border-box pricing-box">
                    <div class="pricing-head-content py-4 px-3">
                      <span class="d-block text-uppercase pb-1">Tier One</span>
                      <h3 class="md-headline d-block text-primary pb-1 my-0">$ 100</h3>
                      <p class="md-body-1">Best for individual agents</p>
                    </div>
                    <div class="pricing-body-content py-3 px-3">
                      <md-list class="md-listing">
                        <md-list-item class="position-relative">
                          <md-icon
                            class="md-icon md-icon-font md-theme-default position-absolute"
                          >done</md-icon>
                          <span class="md-list-item-text">Item One</span>
                        </md-list-item>
                        <md-list-item class="position-relative">
                          <md-icon
                            class="md-icon md-icon-font md-theme-default position-absolute"
                          >done</md-icon>
                          <span class="md-list-item-text">Item Two</span>
                        </md-list-item>
                        <md-list-item class="position-relative">
                          <md-icon
                            class="md-icon md-icon-font md-theme-default position-absolute"
                          >done</md-icon>
                          <span class="md-list-item-text">Item Three</span>
                        </md-list-item>
                      </md-list>
                      <button
                        class="md-button md-primary md-theme-default button-custom-regular button-outline"
                      >Buy now</button>
                    </div>
                  </div>
                </div>
                <div
                  class="md-layout-item md-medium-size-33 md-small-size-50 md-xsmall-size-100 mb-3"
                >
                  <div class="md-card-custom border-box pricing-box">
                    <div class="pricing-head-content py-4 px-3">
                      <span class="d-block text-uppercase pb-1">Tier One</span>
                      <h3 class="md-headline d-block text-primary pb-1 my-0">$ 100</h3>
                      <p class="md-body-1">Best for individual agents</p>
                    </div>
                    <div class="pricing-body-content py-3 px-3">
                      <md-list class="md-listing">
                        <md-list-item class="position-relative">
                          <md-icon
                            class="md-icon md-icon-font md-theme-default position-absolute"
                          >done</md-icon>
                          <span class="md-list-item-text">Item One</span>
                        </md-list-item>
                        <md-list-item class="position-relative">
                          <md-icon
                            class="md-icon md-icon-font md-theme-default position-absolute"
                          >done</md-icon>
                          <span class="md-list-item-text">Item Two</span>
                        </md-list-item>
                        <md-list-item class="position-relative">
                          <md-icon
                            class="md-icon md-icon-font md-theme-default position-absolute"
                          >done</md-icon>
                          <span class="md-list-item-text">Item Three</span>
                        </md-list-item>
                      </md-list>
                      <button
                        class="md-button md-primary md-theme-default button-custom-regular button-outline"
                      >Buy now</button>
                    </div>
                  </div>
                </div>
              </div>
            </md-tab>
            <md-tab id="tab-pages" md-label="Annual">
              <div class="md-layout md-gutter">
                <div
                  class="md-layout-item md-medium-size-33 md-small-size-50 md-xsmall-size-100 mb-3"
                >
                  <div class="md-card-custom border-box pricing-box">
                    <div class="pricing-head-content py-4 px-3">
                      <span class="d-block text-uppercase pb-1">Tier One</span>
                      <h3 class="md-headline d-block text-primary pb-1 my-0">$ 100</h3>
                      <p class="md-body-1">Best for individual agents</p>
                    </div>
                    <div class="pricing-body-content py-3 px-3">
                      <md-list class="md-listing">
                        <md-list-item class="position-relative">
                          <md-icon
                            class="md-icon md-icon-font md-theme-default position-absolute"
                          >done</md-icon>
                          <span class="md-list-item-text">Item One</span>
                        </md-list-item>
                        <md-list-item class="position-relative">
                          <md-icon
                            class="md-icon md-icon-font md-theme-default position-absolute"
                          >done</md-icon>
                          <span class="md-list-item-text">Item Two</span>
                        </md-list-item>
                        <md-list-item class="position-relative">
                          <md-icon
                            class="md-icon md-icon-font md-theme-default position-absolute text-disabled"
                          >close</md-icon>
                          <span class="md-list-item-text text-disabled">Item Three</span>
                        </md-list-item>
                      </md-list>
                      <button
                        class="md-button md-primary md-theme-default button-custom-regular button-outline"
                      >Buy now</button>
                    </div>
                  </div>
                </div>
                <div
                  class="md-layout-item md-medium-size-33 md-small-size-50 md-xsmall-size-100 mb-3"
                >
                  <div class="md-card-custom border-box pricing-box">
                    <div class="pricing-head-content py-4 px-3">
                      <span class="d-block text-uppercase pb-1">Tier One</span>
                      <h3 class="md-headline d-block text-primary pb-1 my-0">$ 100</h3>
                      <p class="md-body-1">Best for individual agents</p>
                    </div>
                    <div class="pricing-body-content py-3 px-3">
                      <md-list class="md-listing">
                        <md-list-item class="position-relative">
                          <md-icon
                            class="md-icon md-icon-font md-theme-default position-absolute"
                          >done</md-icon>
                          <span class="md-list-item-text">Item One</span>
                        </md-list-item>
                        <md-list-item class="position-relative">
                          <md-icon
                            class="md-icon md-icon-font md-theme-default position-absolute"
                          >done</md-icon>
                          <span class="md-list-item-text">Item Two</span>
                        </md-list-item>
                        <md-list-item class="position-relative">
                          <md-icon
                            class="md-icon md-icon-font md-theme-default position-absolute"
                          >done</md-icon>
                          <span class="md-list-item-text">Item Three</span>
                        </md-list-item>
                      </md-list>
                      <button
                        class="md-button md-primary md-theme-default button-custom-regular button-outline"
                      >Buy now</button>
                    </div>
                  </div>
                </div>
                <div
                  class="md-layout-item md-medium-size-33 md-small-size-50 md-xsmall-size-100 mb-3"
                >
                  <div class="md-card-custom border-box pricing-box">
                    <div class="pricing-head-content py-4 px-3">
                      <span class="d-block text-uppercase pb-1">Tier One</span>
                      <h3 class="md-headline d-block text-primary pb-1 my-0">$ 100</h3>
                      <p class="md-body-1">Best for individual agents</p>
                    </div>
                    <div class="pricing-body-content py-3 px-3">
                      <md-list class="md-listing">
                        <md-list-item class="position-relative">
                          <md-icon
                            class="md-icon md-icon-font md-theme-default position-absolute"
                          >done</md-icon>
                          <span class="md-list-item-text">Item One</span>
                        </md-list-item>
                        <md-list-item class="position-relative">
                          <md-icon
                            class="md-icon md-icon-font md-theme-default position-absolute"
                          >done</md-icon>
                          <span class="md-list-item-text">Item Two</span>
                        </md-list-item>
                        <md-list-item class="position-relative">
                          <md-icon
                            class="md-icon md-icon-font md-theme-default position-absolute"
                          >done</md-icon>
                          <span class="md-list-item-text">Item Three</span>
                        </md-list-item>
                      </md-list>
                      <button
                        class="md-button md-primary md-theme-default button-custom-regular button-outline"
                      >Buy now</button>
                    </div>
                  </div>
                </div>
              </div>
            </md-tab>
          </md-tabs>
        </div>
      </section>
      <section class="d-block mb-3 w-100">
        <h1>Input</h1>
        <div class="text-left">
          <md-field>
            <label>E-mail address</label>
            <md-input type="email" />
          </md-field>
          <md-field class="password-eye pr-4">
            <label>Password</label>
            <md-input type="password" />
          </md-field>
          <md-checkbox class>Remember me</md-checkbox>
        </div>
      </section>
      <section class="d-block mb-3 w-100">
        <h1>Button</h1>
        <button class="md-button md-primary md-theme-default button-custom-regular">Buy now</button>
        <button
          class="md-button md-primary md-theme-default button-custom-regular button-outline"
        >Buy now</button>
        <a href="#/auth/login" class="cancel-link text-underline mt-4 d-block">Go to login page</a>
        <md-button :md-ripple="false" class="cancel-link lighter-description mt-2">Cancel</md-button>
        <button
          type="submit"
          class="md-button md-primary md-theme-default button-custom-regular d-flex align-center justify-content-center"
        >
          <LoadingButtonLoader
            :typeEnable="'simpleLoader'"
            v-bind:enable="loading"
            class="mr-1 d-inline-block v-align-middle"
          />
          <span class>Submit</span>
        </button>
        <md-button class="md-primary md-icon-button" @click="showStep = true">
          <md-icon>add</md-icon>
        </md-button>
        <md-button class="md-primary md-icon-button" @click="addTemplate = true">
          <md-icon>add</md-icon>
        </md-button>
        <md-button class="md-primary" @click="deleteAgent = true">Delete agent</md-button>
      </section>
      <section class="d-block mb-3 w-100">
        <h1>Table</h1>
        <div class="full-page-loader">
          <LoadingBar :typeEnable="app.checkType('simpleLoader')" v-bind:enable="app.loading" />
        </div>
        <md-table class="mb-3" v-model="users" md-sort="name" md-sort-order="asc">
          <md-table-row slot="md-table-row" slot-scope="{ item }">
            <md-table-cell md-label="Name" class="avatar-cell">
              <div class="md-avatar position-absolute">
                <img src="https://placeimg.com/40/40/people/1" alt="People" />
              </div>
              <div class="md-avatar md-intial position-absolute">RH</div>
              <span>
                {{ item.name }}
                <label class="review-label">
                  4
                  <md-icon>star</md-icon>
                </label>
              </span>
            </md-table-cell>
            <md-table-cell md-label="Email Address">{{ item.emailaddress }}</md-table-cell>
            <md-table-cell md-label="Phone Number">{{ item.phonenumber }}</md-table-cell>
            <md-table-cell md-label="Role">{{ item.role }}</md-table-cell>
            <md-table-cell md-label="Title">{{ item.title }}</md-table-cell>
            <md-table-cell md-label="Access">{{ item.access }}</md-table-cell>
            <md-table-cell md-label>
              <md-menu md-direction="bottom-end">
                <md-button md-menu-trigger class="md-icon-button icon-button-flat">
                  <md-icon>more_vert</md-icon>
                </md-button>
                <md-menu-content class="menu-content-custom">
                  <md-menu-item>Edit</md-menu-item>
                  <md-menu-item>Delete</md-menu-item>
                </md-menu-content>
              </md-menu>
            </md-table-cell>
          </md-table-row>
        </md-table>
        <div class="md-layout">
          <div
            class="md-layout-item md-medium-size-50 md-xsmall-size-100 md-size-50 d-flex align-center"
          >
            <p class="description font-weight-medium">Displaying 7 of 20 records</p>
          </div>
          <div class="md-layout-item md-medium-size-50 md-xsmall-size-100 md-size-50">
            <!-- for pagination you can check vue material kit -->
            <ul class="pagination">
              <!-- <li class="page-item prev-page">
                        <a class="page-link" aria-label="Previous">
                            <template v-if="withText">Prev</template>
                            <i class="fas fa-angle-double-left" v-else></i>
                        </a>
              </li>-->
              <li class="page-item active">
                <a class="page-link">1</a>
              </li>
              <li class="page-item">
                <a class="page-link">2</a>
              </li>
              <li class="page-item">
                <a class="page-link">3</a>
              </li>
              <li class="page-item">
                <a class="page-link">4</a>
              </li>
              <li class="page-item page-pre next-page">
                <a class="page-link" aria-label="Next">
                  <span class="pl-1">Next page</span>
                  <i class="fas fa-angle-double-right"></i>
                </a>
              </li>
            </ul>
          </div>
        </div>
        <div class="d-block w-100 my-4 content-wrapper">
          <div class="md-layout flex-md-column-reverse">
            <div class="md-layout-item md-medium-size-100 md-xsmall-size-100 md-size-100">
              <md-card class="md-card-plain md-card-custom custom-shadow radius-10 mt-0">
                <md-card-header class="px-4 mb-0 mt-0">
                  <div class="md-headline">
                    <span class="text-primary">Transaction Settings</span>
                    <span class="brand-primary">
                      <md-icon class="brand-primary">keyboard_arrow_right</md-icon>Templates
                    </span>
                  </div>
                </md-card-header>
                <md-card-content class="px-4 py-3">
                  <div class="md-layout">
                    <div class="md-layout-item d-flex justify-content-between mb-3">
                      <div class="d-flex align-center justify-start">
                        <label
                          for
                          class="mr-3 active-label description d-flex align-center justify-content-center"
                        >Active</label>
                        <h3 class="text-primary font-20 my-0">Active Templates</h3>
                      </div>
                      <md-button class="md-primary md-icon-button" @click="showDialog = true">
                        <md-icon>add</md-icon>
                      </md-button>
                    </div>
                  </div>

                  <md-table
                    class="mb-3 active-template"
                    v-model="activetemplates"
                    md-sort="name"
                    md-sort-order="asc"
                  >
                    <md-table-row slot="md-table-row" slot-scope="{ item }">
                      <md-table-cell md-label>{{ item.templatename }}</md-table-cell>
                      <md-table-cell md-label="Template Steps" class="steps">
                        <md-list>
                          <md-list-item>Step 1</md-list-item>
                          <md-list-item>Step 2</md-list-item>
                          <md-list-item>Step 3</md-list-item>
                        </md-list>
                      </md-table-cell>
                      <md-table-cell md-label="Created">{{ item.created }}</md-table-cell>
                      <md-table-cell md-label class="justify-content-end">
                        <md-menu md-direction="bottom-end">
                          <md-button md-menu-trigger class="md-icon-button icon-button-flat">
                            <md-icon>more_vert</md-icon>
                          </md-button>
                          <md-menu-content class="menu-content-custom">
                            <md-menu-item>Edit</md-menu-item>
                            <md-menu-item>Delete</md-menu-item>
                          </md-menu-content>
                        </md-menu>
                      </md-table-cell>
                    </md-table-row>
                  </md-table>
                  <div class="md-layout">
                    <div
                      class="md-layout-item md-medium-size-50 md-xsmall-size-100 md-size-50 d-flex align-center"
                    >
                      <p class="description font-weight-medium">Displaying 10 of 20 records</p>
                    </div>
                    <div
                      class="md-layout-item md-medium-size-50 md-xsmall-size-100 md-size-50 justify-content-end d-flex"
                    >
                      <md-button class="load-more-button md-primary">
                        <i class="fas fa-sync-alt"></i> Load more
                      </md-button>
                    </div>
                  </div>
                </md-card-content>
              </md-card>
            </div>
          </div>
        </div>
      </section>
      <div class="content-wrapper">
        <!-- Transaction list -->
        <div class="md-layout flex-md-column-reverse">
          <div class="md-layout-item md-medium-size-100 md-xsmall-size-100 md-size-75">
            <md-card class="md-card-plain md-card-custom custom-shadow radius-10 mt-0">
              <md-card-header class="px-4 mb-0 mt-0">
                <div class="md-headline d-flex align-center justify-content-between">
                  <span class="text-primary">Transaction</span>
                  <md-button class="md-primary button-custom-regular heading-add-button">
                    <md-icon class="mr-1">add</md-icon>Add transaction
                  </md-button>
                </div>
              </md-card-header>
              <md-card-content class="px-4 py-3">
                <md-table class="mb-3 transaction-table" v-model="transactionList">
                  <md-table-row class="table-head">
                    <md-table-head>Transaction</md-table-head>
                    <md-table-head>Address</md-table-head>
                    <md-table-head>First Name</md-table-head>
                    <md-table-head>Last Name</md-table-head>
                    <md-table-head>Status</md-table-head>
                    <md-table-head>Type</md-table-head>
                    <md-table-head>Created</md-table-head>
                    <md-table-head>Agent(s)</md-table-head>
                  </md-table-row>
                  <md-table-row slot="md-table-row" v-for="item in transactionList" :key="item.id">
                    <md-table-cell class="brand-primary">{{ item.transaction }}</md-table-cell>
                    <md-table-cell class="avatar-cell">
                      <div class="md-avatar position-absolute">
                        <img src="https://placeimg.com/40/40/people/1" alt="People" />
                      </div>
                      <div>
                        <span class="d-block w-100 mb-1">{{ item.addressStreet }}</span>
                        <span class="description">
                          {{ item.addressCity }}
                          <label class="review-label">
                            4
                            <md-icon>star</md-icon>
                          </label>
                        </span>
                      </div>
                    </md-table-cell>
                    <md-table-cell>
                      <span class="text-primary">{{ item.firstName }}</span>
                    </md-table-cell>
                    <md-table-cell>
                      <span class="text-primary">{{ item.lastName }}</span>
                    </md-table-cell>
                    <md-table-cell>
                      <label
                        for
                        class="mr-3 active-label description d-flex align-center justify-content-center"
                      >Active</label>
                      <label
                        for
                        class="mr-3 closed-label description d-flex align-center justify-content-center"
                      >Closed</label>
                    </md-table-cell>
                    <md-table-cell>Buyer</md-table-cell>
                    <md-table-cell>{{ item.created }}</md-table-cell>
                    <md-table-cell>
                      <md-list class="agents-ul">
                        <md-list-item>
                          <div class="md-avatar">
                            <img src="https://placeimg.com/40/40/people/1" alt />
                          </div>
                        </md-list-item>
                        <md-list-item>
                          <div class="md-avatar">
                            <img src="https://placeimg.com/40/40/people/1" alt />
                          </div>
                        </md-list-item>
                        <md-list-item>
                          <div class="md-avatar">
                            <img src="https://placeimg.com/40/40/people/1" alt />
                          </div>
                        </md-list-item>
                        <md-list-item>
                          <div class="md-avatar md-initial">
                            <span>RT</span>
                          </div>
                        </md-list-item>
                      </md-list>
                    </md-table-cell>
                  </md-table-row>
                </md-table>
                <div class="md-layout">
                  <div
                    class="md-layout-item md-medium-size-50 md-xsmall-size-100 md-size-50 d-flex align-center"
                  >
                    <p class="description font-weight-medium">Displaying 10 of 20 records</p>
                  </div>
                  <div class="md-layout-item md-medium-size-50 md-xsmall-size-100 md-size-50">
                    <!-- for pagination you can check vue material kit -->
                    <ul class="pagination">
                      <!-- <li class="page-item prev-page">
                                <a class="page-link" aria-label="Previous">
                                    <template v-if="withText">Prev</template>
                                    <i class="fas fa-angle-double-left" v-else></i>
                                </a>
                      </li>-->
                      <li class="page-item active">
                        <a class="page-link">1</a>
                      </li>
                      <li class="page-item">
                        <a class="page-link">2</a>
                      </li>
                      <li class="page-item">
                        <a class="page-link">3</a>
                      </li>
                      <li class="page-item">
                        <a class="page-link">4</a>
                      </li>
                      <li class="page-item page-pre next-page">
                        <a class="page-link" aria-label="Next">
                          <span class="pl-1">Next page</span>
                          <i class="fas fa-angle-double-right"></i>
                        </a>
                      </li>
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
                <img
                  :src="'../img/search-icon.svg'"
                  alt
                  style="width: 20px;"
                  class="position-absolute"
                />
                <label>Search</label>
                <md-input v-on:keyup.native="doFilter" class="search-input"></md-input>
              </md-field>
              <div class="mx-auto filter-section-outer">
                <div
                  class="d-flex justify-content-between align-center mb-3 filter-heading-section"
                >
                  <h4 class="md-headline text-primary font-weight-medium my-0 hide-md">Filter</h4>
                  <md-icon class="display-md">filter_list</md-icon>
                  <small class="lighter-description font-weight-medium">Reset filter</small>
                </div>
                <div class="filter-section">
                  <div>
                    <div class="d-flex justify-content-start">
                      <md-icon class="text-primary">date_range</md-icon>
                      <h4 class="description text-primary w-100 pl-2">Created date</h4>
                    </div>
                    <div class="d-flex justify-content-between align-center">
                      <p
                        class="brand-primary font-weight-semi-bold mb-0 pl-4"
                      >Aug 01, 2019 - Dec 22, 2019</p>
                      <md-icon
                        class="cursor-pointer closed-label mr-3 closed-label close-label-circle description d-flex align-center justify-content-center"
                      >close</md-icon>
                    </div>

                    <hr />
                  </div>
                  <div>
                    <div class="d-flex justify-content-start">
                      <md-icon class="text-primary">inbox</md-icon>
                      <h4 class="description text-primary w-100 pl-2">Access</h4>
                    </div>
                    <div class="d-flex justify-content-between align-center">
                      <md-checkbox class>Normal</md-checkbox>
                      <label for class="label-account">295</label>
                    </div>
                    <div class="d-flex justify-content-between align-center">
                      <md-checkbox class>Disable</md-checkbox>
                      <label for class="label-account">236</label>
                    </div>
                    <hr />
                  </div>
                  <div>
                    <div class="d-flex justify-content-start">
                      <md-icon class="text-primary">people_alt</md-icon>
                      <h4 class="description text-primary w-100 pl-2">By Role</h4>
                    </div>
                    <div class="d-flex justify-content-between align-center">
                      <md-checkbox class>Agent</md-checkbox>
                      <label for class="label-account">20</label>
                    </div>
                    <div class="d-flex justify-content-between align-center">
                      <md-checkbox class>Co-ordinator</md-checkbox>
                      <label for class="label-account">35</label>
                    </div>
                    <hr />
                  </div>
                  <div>
                    <div class="d-flex justify-content-start">
                      <md-icon class="text-primary">star</md-icon>
                      <h4 class="description text-primary w-100 pl-2">By Rating</h4>
                    </div>
                    <div class="d-flex justify-content-between align-center">
                      <span>
                        <md-checkbox>
                          1
                          <md-icon class="star"></md-icon>
                        </md-checkbox>
                        <md-checkbox>
                          2
                          <md-icon class="star"></md-icon>
                        </md-checkbox>
                        <md-checkbox>
                          3
                          <md-icon class="star"></md-icon>
                        </md-checkbox>
                        <md-checkbox>
                          4
                          <md-icon class="star"></md-icon>
                        </md-checkbox>
                        <md-checkbox>
                          5
                          <md-icon class="star"></md-icon>
                        </md-checkbox>
                      </span>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
        <!-- Transaction list detail -->
        <div class="md-layout flex-md-column-reverse">
          <div class="md-layout-item md-medium-size-100 md-xsmall-size-100 md-size-75">
            <md-card class="md-card-plain md-card-custom custom-shadow radius-10 mt-0">
              <md-card-header class="px-4 mb-0 mt-0">
                <div class="md-headline d-flex align-center justify-content-between">
                  <span class="brand-primary">#02313</span>
                  <label
                    for
                    class="mr-3 active-label description d-flex align-center justify-content-center"
                  >Active</label>
                </div>
              </md-card-header>
              <md-card-content class="px-4 py-3">
                <!-- Public details start -->
                <section class="mb-5">
                  <div class="md-layout flex-md-column-reverse">
                    <div class="md-layout-item md-medium-size-100 md-xsmall-size-100 md-size-60">
                      <div class="d-flex align-center mb-3">
                        <h4 class="md-title text-primary mr-3">Public Details</h4>
                        <span
                          class="cursor-pointer brand-primary"
                          @click="publicDetails = true"
                        >Edit</span>
                      </div>
                      <p class="text-primary description">
                        Lorem ipsum dolor sit amet, consectetur adipiscing elit. Vestibulum blandit at velit non sollicitudin. Mauris ligula ex, luctus quis diam quis, pharetra convallis augue. Mauris nec iaculis tortor, nec porta lacus. Donec pulvinar nisl id enim viverra auctor.
                        Morbi non massa eget ante hendrerit fermentum. Maecenas fermentum dui nec elit elementum, sit amet eleifend sapien tempor.
                      </p>
                    </div>
                    <div class="md-layout-item md-medium-size-100 md-xsmall-size-100 md-size-40">
                      <div style="--aspect-ratio: 16/9;">
                        <iframe
                          width="560"
                          height="315"
                          src="https://www.youtube.com/embed/Wy9q22isx3U"
                          frameborder="0"
                          allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture"
                          allowfullscreen
                        ></iframe>
                      </div>
                    </div>
                  </div>
                </section>
                <!-- Public details end -->
                <!-- Agent list start -->
                <section class="mb-5">
                  <div class="md-layout">
                    <div class="md-layout-item d-flex justify-content-between mb-3">
                      <h3 class="md-title text-primary">Agent(s)</h3>
                      <md-button class="md-primary md-icon-button" @click="addAgents = true">
                        <md-icon>add</md-icon>
                      </md-button>
                    </div>
                  </div>
                  <md-table class="mb-3 table-agent" v-model="agentList">
                    <md-table-row class="table-head">
                      <md-table-head></md-table-head>
                      <md-table-head>First Name</md-table-head>
                      <md-table-head>Last Name</md-table-head>
                      <md-table-head>Email Address</md-table-head>
                      <md-table-head>Phone Number</md-table-head>
                      <md-table-head>Title</md-table-head>
                      <md-table-head>Commission Earned</md-table-head>
                      <md-table-head></md-table-head>
                    </md-table-row>
                    <md-table-row v-for="item in agentList" :key="item.id">
                      <md-table-cell>
                        <div class="md-avatar">
                          <img src="https://placeimg.com/40/40/people/1" alt="People" />
                        </div>
                      </md-table-cell>
                      <md-table-cell>{{ item.firstName }}</md-table-cell>
                      <md-table-cell>{{ item.lastName }}</md-table-cell>
                      <md-table-cell>{{ item.email }}</md-table-cell>
                      <md-table-cell>{{ item.phone }}</md-table-cell>
                      <md-table-cell>{{ item.title }}</md-table-cell>
                      <md-table-cell>
                        <span
                          class="text-primary md-title font-weight-semi-bold"
                        >{{ item.commision }}</span>
                      </md-table-cell>
                      <md-table-cell md-label>
                        <md-menu md-direction="bottom-end">
                          <md-button md-menu-trigger class="md-icon-button icon-button-flat">
                            <md-icon>more_vert</md-icon>
                          </md-button>
                          <md-menu-content class="menu-content-custom">
                            <md-menu-item @click="editAgents = true">Edit</md-menu-item>
                            <md-menu-item @click="deleteAgent = true">Delete</md-menu-item>
                          </md-menu-content>
                        </md-menu>
                      </md-table-cell>
                    </md-table-row>
                  </md-table>
                </section>
                <!-- Agent list end -->
                <!-- Transaction users start -->
                <section class="mb-5">
                  <div class="md-layout">
                    <div class="md-layout-item d-flex justify-content-between mb-3">
                      <h3 class="md-title text-primary">Transaction users</h3>
                      <div class="d-flex align-center">
                        <md-button class="md-secondary md-icon-button">
                          <md-icon>autorenew</md-icon>
                        </md-button>
                        <md-button
                          class="md-primary md-icon-button"
                          @click="addTransactionUser = true"
                        >
                          <md-icon>add</md-icon>
                        </md-button>
                      </div>
                    </div>
                  </div>
                  <md-table class="mb-3 table-transaction-user" v-model="userList">
                    <md-table-row class="table-head">
                      <md-table-head>First Name</md-table-head>
                      <md-table-head>Last Name</md-table-head>
                      <md-table-head>Email Address</md-table-head>
                      <md-table-head>Phone Number</md-table-head>
                      <md-table-head>Role</md-table-head>
                      <md-table-head>Allocated Steps</md-table-head>
                      <md-table-head>Access Code</md-table-head>
                    </md-table-row>
                    <md-table-row v-for="item in userList" :key="item.id">
                      <md-table-cell>{{ item.firstName }}</md-table-cell>
                      <md-table-cell>{{ item.lastName }}</md-table-cell>
                      <md-table-cell>{{ item.email }}</md-table-cell>
                      <md-table-cell>{{ item.phone }}</md-table-cell>
                      <md-table-cell>{{ item.role }}</md-table-cell>
                      <md-table-cell class="steps">
                        <md-list>
                          <md-list-item>Step 1</md-list-item>
                          <md-list-item>Step 2</md-list-item>
                        </md-list>
                      </md-table-cell>
                      <md-table-cell>{{ item.access }}</md-table-cell>
                    </md-table-row>
                  </md-table>
                </section>
                <!-- Transaction users end -->
                <!-- Transaction Vendors start -->
                <section class="mb-5">
                  <div class="md-layout">
                    <div class="md-layout-item d-flex justify-content-between mb-3">
                      <h3 class="md-title text-primary">vendor(s)</h3>
                      <md-button class="md-primary md-icon-button">
                        <md-icon>add</md-icon>
                      </md-button>
                    </div>
                  </div>
                  <md-table class="mb-3 table-agent" v-model="vendorList">
                    <md-table-row class="table-head">
                      <md-table-head></md-table-head>
                      <md-table-head>First Name</md-table-head>
                      <md-table-head>Last Name</md-table-head>
                      <md-table-head>Email Address</md-table-head>
                      <md-table-head>Phone Number</md-table-head>
                      <md-table-head>Company</md-table-head>
                      <md-table-head></md-table-head>
                    </md-table-row>
                    <md-table-row v-for="item in vendorList" :key="item.id">
                      <md-table-cell>
                        <div class="md-avatar">
                          <img src="https://placeimg.com/40/40/people/1" alt="People" />
                        </div>
                      </md-table-cell>
                      <md-table-cell>{{ item.firstName }}</md-table-cell>
                      <md-table-cell>
                        <span class="text-primary">{{ item.lastName }}</span>
                      </md-table-cell>
                      <md-table-cell>{{ item.email }}</md-table-cell>
                      <md-table-cell>{{ item.phone }}</md-table-cell>
                      <md-table-cell>{{ item.company }}</md-table-cell>
                      <md-table-cell md-label>
                        <md-menu md-direction="bottom-end">
                          <md-button md-menu-trigger class="md-icon-button icon-button-flat">
                            <md-icon>more_vert</md-icon>
                          </md-button>
                          <md-menu-content class="menu-content-custom">
                            <md-menu-item>Edit</md-menu-item>
                            <md-menu-item>Delete</md-menu-item>
                          </md-menu-content>
                        </md-menu>
                      </md-table-cell>
                    </md-table-row>
                  </md-table>
                </section>
                <!-- Transaction Vendors end -->
                <!-- our template -->
                <section id="app" class="section">
                  <div class="md-layout">
                    <div class="md-layout-item d-flex justify-content-between mb-3">
                      <h3 class="md-title text-primary">Transaction Steps</h3>
                      <div class="d-flex align-center">
                        <p
                          class="text-primary mr-3 mb-0 cursor-pointer"
                          @click="importTemplate = true"
                        >
                          <md-icon class="text-primary mr-1">get_app</md-icon>Import Template
                        </p>
                        <md-button class="md-primary md-icon-button">
                          <md-icon>add</md-icon>
                        </md-button>
                      </div>
                    </div>
                  </div>
                  <ul class="transaction-step-timeline">
                    <li class="active">
                      <div class="step-icon">
                        <span>1</span>
                      </div>
                      <article class="message transaction-step w-100" :class="accordionClasses">
                        <div class="message-header px-4 py-3" @click="toggleAccordion">
                          <div class="d-flex justify-content-between align-center">
                            <h4 class="my-1">Step 1</h4>
                            <div>
                              <p class="my-1">Est. Due Date: Sep12, 2019</p>
                            </div>
                          </div>
                        </div>
                        <div class="message-body custom-shadow">
                          <div class="message-content">
                            <div class="md-layout">
                              <div class="md-layout-item md-size-60">
                                <div class="md-layout">
                                  <div class="md-layout-item md-size-33">
                                    <label for>Title</label>
                                    <p class="text-primary">Name of step</p>
                                  </div>
                                  <div class="md-layout-item md-size-33">
                                    <label for>Agent</label>
                                    <p class="text-primary">
                                      alex@lopez.com
                                      <span class="brand-primary">+2</span>
                                    </p>
                                  </div>
                                  <div class="md-layout-item md-size-33">
                                    <label for>Transaction user</label>
                                    <p class="text-primary">
                                      adam@smith.com
                                      <span class="brand-primary">+2</span>
                                    </p>
                                  </div>
                                  <div class="md-layout-item md-size-33">
                                    <label for>video</label>
                                    <p class="brand-primary">http://repylot.com</p>
                                  </div>
                                </div>
                              </div>
                              <div class="md-layout-item md-size-40">
                                <div class="md-layout-item md-size-100">
                                  <label for>Description</label>
                                  <p class="text-primary">
                                    Lorem ipsum dolor sit amet, consectetur adipiscing elit. Vestibulum blandit at velit non sollicitudin. Mauris ligula ex, luctus quis diam quis, pharetra convallis
                                    <span
                                      class="brand-primary"
                                    >read more</span>
                                  </p>
                                </div>
                                <div
                                  class="md-layout-item md-size-100 d-flex align-center justify-content-between"
                                >
                                  <div
                                    class="d-flex justify-content-between align-center"
                                    @click="enableConfirmation = true"
                                  >
                                    <md-checkbox class>Enable the option for confirmation</md-checkbox>
                                  </div>
                                  <div class="transaction-steps-option">
                                    <span
                                      class="cursor-pointer"
                                      @click="transactionStepsEdit = true"
                                    >
                                      <md-icon>edit</md-icon>
                                    </span>
                                    <span class="cursor-pointer ml-2" @click="deleteAgent = true">
                                      <img :src="'../img/trashcan.svg'" alt />
                                    </span>
                                  </div>
                                </div>
                              </div>
                            </div>
                          </div>
                        </div>
                      </article>
                    </li>
                    <li>
                      <div class="step-icon">
                        <span>2</span>
                      </div>
                      <article class="message transaction-step w-100" :class="accordionClasses">
                        <div class="message-header px-4 py-3" @click="toggleAccordion">
                          <div class="d-flex justify-content-between align-center">
                            <h4 class="my-1">Step 2</h4>
                            <div>
                              <p class="my-1 text-primary">
                                Est. Due Date:
                                <span class="description">Sep12, 2019</span>
                              </p>
                            </div>
                          </div>
                        </div>
                        <div class="message-body custom-shadow">
                          <div class="message-content">
                            <div class="md-layout">
                              <div class="md-layout-item md-size-60">
                                <div class="md-layout">
                                  <div class="md-layout-item md-size-33">
                                    <label for>Title</label>
                                    <p class="text-primary">Name of step</p>
                                  </div>
                                  <div class="md-layout-item md-size-33">
                                    <label for>Agent</label>
                                    <p class="text-primary">
                                      alex@lopez.com
                                      <span class="brand-primary">+2</span>
                                    </p>
                                  </div>
                                  <div class="md-layout-item md-size-33">
                                    <label for>Transaction user</label>
                                    <p class="text-primary">
                                      adam@smith.com
                                      <span class="brand-primary">+2</span>
                                    </p>
                                  </div>
                                  <div class="md-layout-item md-size-33">
                                    <label for>video</label>
                                    <p class="brand-primary">http://repylot.com</p>
                                  </div>
                                </div>
                              </div>
                              <div class="md-layout-item md-size-40">
                                <div class="md-layout-item md-size-100">
                                  <label for>Description</label>
                                  <p class="text-primary">
                                    Lorem ipsum dolor sit amet, consectetur adipiscing elit. Vestibulum blandit at velit non sollicitudin. Mauris ligula ex, luctus quis diam quis, pharetra convallis
                                    <span
                                      class="brand-primary"
                                    >read more</span>
                                  </p>
                                </div>
                                <div
                                  class="md-layout-item md-size-100 d-flex align-center justify-content-between"
                                >
                                  <div class="d-flex justify-content-between align-center">
                                    <md-checkbox class>Enable the option for confirmation</md-checkbox>
                                  </div>
                                  <div class="transaction-steps-option">
                                    <md-icon
                                      class="cursor-pointer"
                                      @click="transactionStepsEdit = true"
                                    >edit</md-icon>
                                    <span class="cursor-pointer ml-2" @click="deleteAgent = true">
                                      <img :src="'../img/trashcan.svg'" alt />
                                    </span>
                                  </div>
                                </div>
                              </div>
                            </div>
                          </div>
                        </div>
                      </article>
                    </li>
                    <li>
                      <div class="step-icon">
                        <span>3</span>
                      </div>
                      <article class="message transaction-step w-100" :class="accordionClasses">
                        <div class="message-header px-4 py-3" @click="toggleAccordion">
                          <div class="d-flex justify-content-between align-center">
                            <h4 class="my-1">Step 3</h4>
                            <div>
                              <p class="my-1 text-primary">
                                Est. Due Date:
                                <span class="description">Sep12, 2019</span>
                              </p>
                            </div>
                          </div>
                        </div>
                        <div class="message-body custom-shadow">
                          <div class="message-content">
                            <div class="md-layout">
                              <div class="md-layout-item md-size-60">
                                <div class="md-layout">
                                  <div class="md-layout-item md-size-33">
                                    <label for>Title</label>
                                    <p class="text-primary">Name of step</p>
                                  </div>
                                  <div class="md-layout-item md-size-33">
                                    <label for>Agent</label>
                                    <p class="text-primary">
                                      alex@lopez.com
                                      <span class="brand-primary">+2</span>
                                    </p>
                                  </div>
                                  <div class="md-layout-item md-size-33">
                                    <label for>Transaction user</label>
                                    <p class="text-primary">
                                      adam@smith.com
                                      <span class="brand-primary">+2</span>
                                    </p>
                                  </div>
                                  <div class="md-layout-item md-size-33">
                                    <label for>video</label>
                                    <p class="brand-primary">http://repylot.com</p>
                                  </div>
                                </div>
                              </div>
                              <div class="md-layout-item md-size-40">
                                <div class="md-layout-item md-size-100">
                                  <label for>Description</label>
                                  <p class="text-primary">
                                    Lorem ipsum dolor sit amet, consectetur adipiscing elit. Vestibulum blandit at velit non sollicitudin. Mauris ligula ex, luctus quis diam quis, pharetra convallis
                                    <span
                                      class="brand-primary"
                                    >read more</span>
                                  </p>
                                </div>
                                <div
                                  class="md-layout-item md-size-100 d-flex align-center justify-content-between"
                                >
                                  <div class="d-flex justify-content-between align-center">
                                    <md-checkbox class>Enable the option for confirmation</md-checkbox>
                                  </div>
                                  <div class="transaction-steps-option">
                                    <md-icon class="cursor-pointer">edit</md-icon>
                                    <span class="cursor-pointer ml-2">
                                      <img :src="'../img/trashcan.svg'" alt />
                                    </span>
                                  </div>
                                </div>
                              </div>
                            </div>
                          </div>
                        </div>
                      </article>
                    </li>
                  </ul>
                </section>
              </md-card-content>
            </md-card>
          </div>
          <div class="md-layout-item md-medium-size-100 md-xsmall-size-100 md-size-25">
            <div class="search-right mb-4">
              <div class="mx-auto filter-section-outer">
                <div class="mb-3 filter-icon-button">
                  <md-button class="md-white md-icon-button ml-0">
                    <md-icon>edit</md-icon>
                  </md-button>
                  <md-button class="md-white md-icon-button">
                    <md-icon>autorenew</md-icon>
                  </md-button>
                  <md-button class="md-white md-icon-button" @click="closeTransaction = true">
                    <md-icon>close</md-icon>
                  </md-button>
                </div>
                <div class="d-flex justify-content-between align-end mb-3 filter-heading-section">
                  <h4 class="md-headline text-primary font-weight-medium my-0">Overview</h4>
                  <small
                    class="brand-primary font-weight-medium description-normal cursor-pointer"
                    @click="overviewEdit = true"
                  >Edit</small>
                </div>
                <div class="md-layout">
                  <div class="md-layout-item md-size-50 mb-2">
                    <label for class="description description-normal">Transaction ID</label>
                    <p class="text-primary description">02313</p>
                  </div>
                  <div class="md-layout-item md-size-50 mb-2">
                    <label for class="description description-normal">Transaction Type</label>
                    <p class="text-primary description">Seller</p>
                  </div>
                  <div class="md-layout-item md-size-50 mb-2">
                    <label for class="description description-normal">Sales Price</label>
                    <p class="brand-primary md-title">N/A</p>
                  </div>
                  <div class="md-layout-item md-size-50 mb-2">
                    <label for class="description description-normal">Additional Fees</label>
                    <p class="text-primary description">N/A</p>
                  </div>
                  <div class="md-layout-item md-size-50 mb-2">
                    <label for class="description description-normal">Closed Dates</label>
                    <p class="text-primary description">N/A</p>
                  </div>
                  <div class="md-layout-item md-size-50 mb-2">
                    <label for class="description description-normal">Executed Date</label>
                    <p class="text-primary description">N/A</p>
                  </div>
                  <div class="md-layout-item md-size-50 mb-2">
                    <label for class="description description-normal">Sources</label>
                    <p class="text-success description">
                      <md-icon class="font-11">fiber_manual_record</md-icon>Online
                    </p>
                  </div>
                </div>
              </div>
              <div class="mx-auto filter-section-outer">
                <div class="d-flex justify-content-between align-end mb-3 filter-heading-section">
                  <h4 class="md-headline text-primary font-weight-medium my-0">Details</h4>
                  <small
                    class="brand-primary font-weight-medium description-normal cursor-pointer"
                    @click="editDetails = true"
                  >Edit</small>
                </div>
                <div class="md-card md-card-custom custom-shadow radius-10 mt-0">
                  <div class="md-card-content">
                    <div class="md-layout">
                      <div class="md-layout-item md-size-100 mb-2">
                        <label for class="description description-normal">Full Name</label>
                        <p class="text-primary description">John Doll</p>
                      </div>
                      <div class="md-layout-item md-size-100 mb-2">
                        <label for class="description description-normal">MLS #</label>
                        <p class="text-primary description">123456</p>
                      </div>
                      <div class="md-layout-item md-size-100 mb-2">
                        <label for class="description description-normal">Address</label>
                        <p class="text-primary description">
                          618 Blue Road
                          <br />Knoxville, TN
                        </p>
                      </div>
                      <div class="md-layout-item md-size-100 mb-2">
                        <label for class="description description-normal">
                          Review URL
                          <md-icon></md-icon>
                        </label>
                        <p class="brand-primary description">https://facebook.com/repylot</p>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <!-- add step -->
    <md-dialog :md-active.sync="showStep">
      <md-dialog-title class="modal-title d-block py-3 px-4 bg-light position-relative">
        <h4 class="my-0 md-headline text-left text-primary">Add Step</h4>
        <md-button @click="showStep = false" class="md-icon-button position-absolute">
          <md-icon class="text-primary">close</md-icon>
        </md-button>
      </md-dialog-title>
      <div class="md-dialog-content p-0">
        <md-content md-dynamic-height class="px-4">
          <div class="md-layout">
            <div class="md-layout-item md-small-size-100 md-size-50">
              <md-field slot="inputs">
                <label>Step</label>
                <md-select name="role" id="role">
                  <md-option value="admin">Admin</md-option>
                  <md-option value="agent">Agent</md-option>
                  <md-option value="junior-agent">Junior Agent</md-option>
                  <md-option value="senior-agent">Senior Agent</md-option>
                </md-select>
              </md-field>
            </div>
            <div class="md-layout-item md-small-size-100 md-size-50">
              <md-field slot="inputs">
                <label>Step Title</label>
                <md-input></md-input>
              </md-field>
            </div>
            <div class="md-layout-item md-small-size-100 md-size-50">
              <md-field slot="inputs">
                <label>Select a video integration</label>
                <md-select name="role" id="role">
                  <md-option value="admin">Admin</md-option>
                  <md-option value="agent">Agent</md-option>
                  <md-option value="junior-agent">Junior Agent</md-option>
                  <md-option value="senior-agent">Senior Agent</md-option>
                </md-select>
              </md-field>
            </div>
            <div class="md-layout-item md-small-size-100 md-size-50">
              <md-field slot="inputs">
                <label>URL (Vimeo, Youtube, ...)</label>
                <md-input></md-input>
              </md-field>
            </div>
            <div class="md-layout-item md-small-size-100 md-size-100">
              <md-field slot="inputs">
                <label>Description</label>
                <md-textarea
                  value="At vero eos et accusamus et iusto odio dignissimos ducimus qui blanditiis praesentium voluptatum deleniti atque corrupti quos dolores et quas molestias."
                ></md-textarea>
              </md-field>
            </div>
          </div>
        </md-content>
        <md-dialog-actions class="flex-wrap justify-content-center">
          <md-button
            class="md-primary w-100 button-primary-custom mb-3 mt-3"
            @click="showStep = false"
          >Add</md-button>
          <div class="d-block w-100 text-center">
            <md-button
              :md-ripple="false"
              @click="showStep = false"
              class="cancel-link lighter-description mt-2 text-capitalize"
            >Cancel</md-button>
          </div>
        </md-dialog-actions>
      </div>
    </md-dialog>
    <!-- add template -->
    <md-dialog :md-active.sync="addTemplate" class="modal-medium">
      <md-dialog-title class="modal-title d-block py-3 px-4 bg-light position-relative">
        <h4 class="my-0 md-headline text-left text-primary">Add Template</h4>
        <md-button @click="addTemplate = false" class="md-icon-button position-absolute">
          <md-icon class="text-primary">close</md-icon>
        </md-button>
      </md-dialog-title>
      <div class="md-dialog-content p-0">
        <md-content md-dynamic-height class="px-4 mb-4">
          <div class="md-layout">
            <div class="md-layout-item md-small-size-100 md-size-100">
              <md-field>
                <label>Template name</label>
                <md-input></md-input>
              </md-field>
            </div>
          </div>
          <div class="md-layout">
            <div class="md-layout-item md-small-size-100">
              <h4 class="md-title text-left text-primary">Available steps</h4>
              <md-list class="list-add-template">
                <md-list-item class="mb-2">
                  <div
                    class="position-relative flex-wrap justify-content-start d-flex align-center"
                  >
                    <div class="md-avatar position-absolute">AB</div>
                    <p class="md-title text-primary">Step 1</p>
                  </div>
                  <i class="fas fa-angle-double-right description pr-3"></i>
                </md-list-item>
                <md-list-item class="mb-2">
                  <div
                    class="position-relative flex-wrap justify-content-start d-flex align-center"
                  >
                    <div class="md-avatar position-absolute">AB</div>
                    <p class="md-title text-primary">Step 2</p>
                  </div>
                  <i class="fas fa-angle-double-right description pr-3"></i>
                </md-list-item>
              </md-list>
            </div>
            <div class="d-flex align-center justify-content-center mx-auto">
              <div class="outer-swap">
                <div class="inner-swap">
                  <i class="fas fa-angle-double-left text-primary"></i>
                  <i class="fas fa-angle-double-right text-primary"></i>
                </div>
              </div>
            </div>
            <div class="md-layout-item md-small-size-100">
              <h4 class="md-title text-left text-primary">Assigned steps</h4>
              <md-list class="list-add-template assigned-list">
                <md-list-item class="mb-2">
                  <div
                    class="position-relative flex-wrap justify-content-start d-flex align-center"
                  >
                    <div class="md-avatar position-absolute">AB</div>
                    <p class="md-title brand-primary">Step 3</p>
                  </div>
                  <i class="fas fa-angle-double-left description pr-3"></i>
                </md-list-item>
              </md-list>
            </div>
          </div>
        </md-content>
        <md-dialog-actions class="flex-wrap justify-content-center">
          <md-button
            class="md-primary w-100 button-primary-custom mb-2 mt-3"
            @click="addTemplate = false"
          >Add</md-button>
          <div class="d-block w-100 text-center">
            <md-button
              :md-ripple="false"
              @click="addTemplate = false"
              class="cancel-link lighter-description mt-2 mb-2 text-capitalize"
            >Cancel</md-button>
          </div>
        </md-dialog-actions>
      </div>
    </md-dialog>
    <!-- public details edit -->
    <md-dialog :md-active.sync="publicDetails" class="modal-medium">
      <md-dialog-title class="modal-title d-block py-3 px-4 bg-light position-relative">
        <h4 class="my-0 md-headline text-left text-primary">Update public description</h4>
        <md-button @click="publicDetails = false" class="md-icon-button position-absolute">
          <md-icon class="text-primary">close</md-icon>
        </md-button>
      </md-dialog-title>
      <div class="md-dialog-content p-0">
        <md-content md-dynamic-height class="px-4">
          <div class="md-layout">
            <div class="md-layout-item md-small-size-100 md-size-100">
              <md-field slot="inputs">
                <label>Description</label>
                <md-textarea
                  class="pl-0"
                  value="At vero eos et accusamus et iusto odio dignissimos ducimus qui blanditiis praesentium voluptatum deleniti atque corrupti quos dolores et quas molestias."
                ></md-textarea>
              </md-field>
            </div>
            <div class="md-layout-item md-small-size-100 md-size-50">
              <md-field slot="inputs">
                <label>URL (Vimeo, Youtube, ...)</label>
                <md-input></md-input>
              </md-field>
            </div>
            <div class="md-layout-item md-small-size-100 md-size-50">
              <md-field slot="inputs">
                <label>Select a video integration</label>
                <md-select name="role" id="role">
                  <md-option value="admin">Admin</md-option>
                  <md-option value="agent">Agent</md-option>
                  <md-option value="junior-agent">Junior Agent</md-option>
                  <md-option value="senior-agent">Senior Agent</md-option>
                </md-select>
              </md-field>
            </div>
          </div>
        </md-content>
        <md-dialog-actions class="flex-wrap justify-content-center">
          <md-button
            class="md-primary w-100 button-primary-custom mb-1 mt-3"
            @click="publicDetails = false"
          >Add</md-button>
          <div class="d-block w-100 text-center">
            <md-button
              :md-ripple="false"
              @click="publicDetails = false"
              class="cancel-link lighter-description mt-2 mb-3 text-capitalize"
            >Cancel</md-button>
          </div>
        </md-dialog-actions>
      </div>
    </md-dialog>
    <!-- edit details -->
    <md-dialog :md-active.sync="editDetails" class="modal-medium">
      <md-dialog-title class="modal-title d-block py-3 px-4 bg-light position-relative">
        <h4 class="my-0 md-headline text-left text-primary">Edit Address</h4>
        <md-button @click="editDetails = false" class="md-icon-button position-absolute">
          <md-icon class="text-primary">close</md-icon>
        </md-button>
      </md-dialog-title>
      <div class="md-dialog-content p-0">
        <md-content md-dynamic-height class="px-4">
          <div class="md-layout">
            <div class="md-layout-item md-small-size-100 md-size-50">
              <md-field slot="inputs">
                <label>Full name</label>
                <md-input></md-input>
              </md-field>
            </div>
            <div class="md-layout-item md-small-size-100 md-size-50">
              <md-field slot="inputs">
                <label>MLS no.</label>
                <md-input></md-input>
              </md-field>
            </div>
            <div class="md-layout-item md-small-size-100 md-size-50">
              <md-field slot="inputs">
                <label>Street number</label>
                <md-input></md-input>
              </md-field>
            </div>
            <div class="md-layout-item md-small-size-100 md-size-50">
              <md-field slot="inputs">
                <label>Street name</label>
                <md-input></md-input>
              </md-field>
            </div>
            <div class="md-layout-item md-small-size-100 md-size-50">
              <md-field slot="inputs">
                <label>City</label>
                <md-input></md-input>
              </md-field>
            </div>
            <div class="md-layout-item md-small-size-100 md-size-50">
              <md-field slot="inputs">
                <label>State</label>
                <md-select name="role" id="role">
                  <md-option value="California">California</md-option>
                  <md-option value="Florida">Florida</md-option>
                  <md-option value="Texas">Texas</md-option>
                  <md-option value="Hawaii">Hawaii</md-option>
                </md-select>
              </md-field>
            </div>
            <div class="md-layout-item md-small-size-100 md-size-50">
              <md-field slot="inputs">
                <label>Zip</label>
                <md-input></md-input>
              </md-field>
            </div>
            <div class="md-layout-item md-small-size-100 md-size-50">
              <md-field slot="inputs">
                <label>Country</label>
                <md-select name="role" id="role">
                  <md-option value="Europe">Europe</md-option>
                  <md-option value="Albania">Albania</md-option>
                  <md-option value="Croatia">Croatia</md-option>
                  <md-option value="Jordan">Jordan</md-option>
                </md-select>
              </md-field>
            </div>
            <div class="md-layout-item md-small-size-100 md-size-100">
              <md-field slot="inputs">
                <label class="brand-primary">https://facebook.com/repylot</label>
                <md-input></md-input>
              </md-field>
            </div>
          </div>
        </md-content>
        <md-dialog-actions class="flex-wrap justify-content-center">
          <md-button
            class="md-primary w-100 button-primary-custom mb-1 mt-3"
            @click="editDetails = false"
          >Update</md-button>
          <div class="d-block w-100 text-center">
            <md-button
              :md-ripple="false"
              @click="editDetails = false"
              class="cancel-link lighter-description mt-2 mb-3 text-capitalize"
            >Cancel</md-button>
          </div>
        </md-dialog-actions>
      </div>
    </md-dialog>
    <!-- overview edit -->
    <md-dialog :md-active.sync="overviewEdit" class="modal-medium">
      <md-dialog-title class="modal-title d-block py-3 px-4 bg-light position-relative">
        <h4 class="my-0 md-headline text-left text-primary">Add transaction details</h4>
        <md-button @click="overviewEdit = false" class="md-icon-button position-absolute">
          <md-icon class="text-primary">close</md-icon>
        </md-button>
      </md-dialog-title>
      <div class="md-dialog-content p-0">
        <md-content md-dynamic-height class="px-4">
          <div class="md-layout">
            <div class="md-layout-item md-small-size-100 md-size-50">
              <md-field slot="inputs">
                <label>Sales price</label>
                <span class="md-prefix">$</span>
                <md-input value="120,000"></md-input>
              </md-field>
            </div>
            <div class="md-layout-item md-small-size-100 md-size-50">
              <md-field slot="inputs">
                <label>Additional fees</label>
                <md-input></md-input>
              </md-field>
            </div>
            <div class="md-layout-item md-small-size-100 md-size-50">
              <md-field slot="inputs">
                <label>Execute date</label>
                <md-input></md-input>
              </md-field>
            </div>
            <div class="md-layout-item md-small-size-100 md-size-50">
              <md-field slot="inputs">
                <label>Closed date</label>
                <md-input></md-input>
              </md-field>
            </div>
            <div class="md-layout-item md-small-size-100 md-size-50">
              <md-field slot="inputs">
                <label>GCI</label>
                <md-input></md-input>
              </md-field>
            </div>
            <div class="md-layout-item md-small-size-100 md-size-50">
              <md-field slot="inputs">
                <label>Agent review URL</label>
                <md-select name="role" id="role">
                  <md-option value="California">California</md-option>
                  <md-option value="Florida">Florida</md-option>
                  <md-option value="Texas">Texas</md-option>
                  <md-option value="Hawaii">Hawaii</md-option>
                </md-select>
              </md-field>
            </div>
          </div>
        </md-content>
        <md-dialog-actions class="flex-wrap justify-content-center">
          <md-button
            class="md-primary w-100 button-primary-custom mb-1 mt-3"
            @click="overviewEdit = false"
          >Add</md-button>
          <div class="d-block w-100 text-center">
            <md-button
              :md-ripple="false"
              @click="overviewEdit = false"
              class="cancel-link lighter-description mt-2 mb-3 text-capitalize"
            >Cancel</md-button>
          </div>
        </md-dialog-actions>
      </div>
    </md-dialog>
    <!-- add agent -->
    <md-dialog :md-active.sync="addAgents" class="modal-medium">
      <md-dialog-title class="modal-title d-block py-3 px-4 bg-light">
        <h4 class="my-0 md-headline text-left text-primary">Add agent</h4>
      </md-dialog-title>
      <div class="md-dialog-content">
        <form>
          <md-content md-dynamic-height class="px-0">
            <div class="w-100">
              <md-radio v-model="radio" :value="false">
                <span class="text-primary description">New</span>
              </md-radio>
              <md-radio v-model="radio" value="my-radio">
                <span class="text-primary description">Existing</span>
              </md-radio>
            </div>
            <div class="md-layout">
              <div class="md-layout-item md-medium-size-20 md-size-20 md-small-size-100">
                <div class="modal-add-avatar d-flex flex-wrap justify-content-center mt-4">
                  <div class="avatar-dialog d-flex align-center justify-content-center">
                    <md-icon>person_outline</md-icon>
                  </div>
                  <span
                    class="w-100 d-block brand-primary font-weight-regular description-normal text-center"
                  >Profile Avatar</span>
                </div>
              </div>
              <div class="md-layout-item md-medium-size-80 md-size-80 md-small-size-100">
                <div class="md-layout">
                  <div class="md-layout-item md-small-size-100 md-size-50">
                    <md-field>
                      <label>First name</label>
                      <md-input></md-input>
                    </md-field>
                  </div>
                  <div class="md-layout-item md-small-size-100 md-size-50">
                    <md-field>
                      <label>Last name</label>
                      <md-input></md-input>
                    </md-field>
                  </div>
                  <div class="md-layout-item md-small-size-100 md-size-100">
                    <md-field class="multi-error">
                      <label>Email</label>
                      <md-input></md-input>
                    </md-field>
                  </div>
                  <div class="md-layout-item md-small-size-100 md-size-100">
                    <md-field class="multi-error">
                      <label>Phone no.</label>
                      <md-input></md-input>
                    </md-field>
                  </div>
                  <div class="md-layout-item md-small-size-100 md-size-100">
                    <md-field class="multi-error">
                      <label>Agent</label>
                      <md-input value="Agent"></md-input>
                    </md-field>
                  </div>
                  <div class="md-layout-item md-small-size-100 md-size-100">
                    <md-field>
                      <label>Title</label>
                      <md-input></md-input>
                    </md-field>
                  </div>
                </div>
                <md-dialog-actions class="flex-wrap justify-content-start px-0">
                  <md-button type="submit" class="md-primary button-primary-custom mb-3 mt-3 px-4">
                    <span>Add</span>
                  </md-button>

                  <div class="d-block text-center">
                    <md-button
                      :md-ripple="false"
                      @click="addAgents = false"
                      class="cancel-link lighter-description mt-2 text-capitalize"
                    >Cancel</md-button>
                  </div>
                </md-dialog-actions>
              </div>
            </div>
          </md-content>
        </form>
      </div>
    </md-dialog>
    <!-- delete agent -->
    <md-dialog :md-active.sync="deleteAgent" class="modal-medium">
      <md-dialog-title class="modal-title d-block py-3 px-4 bg-light position-relative">
        <h4 class="my-0 md-headline text-left text-primary">Delete</h4>
        <md-button @click="deleteAgent = false" class="md-icon-button position-absolute">
          <md-icon class="text-primary">close</md-icon>
        </md-button>
      </md-dialog-title>
      <div class="md-dialog-content pb-0">
        <form>
          <md-content md-dynamic-height class="px-0">
            <div class="md-layout">
              <div class="md-layout-item md-medium-size-100 md-size-100 md-small-size-100">
                <p class="text-primary description text-center mt-4 mb-3">
                  You have selected to delete
                  <span class="brand-primary">John Doll</span>.
                </p>
                <md-dialog-actions class="flex-wrap justify-content-center">
                  <md-button
                    class="md-danger w-100 button-primary-custom mb-2 mt-3 no-shadow"
                    @click="deleteAgent = false"
                  >Delete</md-button>
                  <div class="d-block w-100 text-center">
                    <md-button
                      :md-ripple="false"
                      @click="deleteAgent = false"
                      class="cancel-link lighter-description mt-2 mb-2 text-capitalize"
                    >Cancel</md-button>
                  </div>
                </md-dialog-actions>
              </div>
            </div>
          </md-content>
        </form>
      </div>
    </md-dialog>
    <!-- edit agent -->
    <md-dialog :md-active.sync="editAgents" class="modal-medium">
      <md-dialog-title class="modal-title d-block py-3 px-4 bg-light">
        <h4 class="my-0 md-headline text-left text-primary">Edit Agents</h4>
      </md-dialog-title>
      <div class="md-dialog-content p-0">
        <form>
          <md-content md-dynamic-height class="px-4">
            <div class="md-layout">
              <div class="md-layout-item md-size-100">
                <div class="md-layout p-30 position-relative">
                  <div class="remove-icon position-absolute">
                    <md-icon>remove</md-icon>
                  </div>
                  <div
                    class="md-layout-item md-small-size-100 md-size-50 video-integration-field position-relative"
                  >
                    <div class="md-avatar position-absolute">
                      <img src="https://placeimg.com/40/40/people/1" alt />
                    </div>
                    <md-field slot="inputs">
                      <label>Select a video integration</label>
                      <md-select name="role" id="role">
                        <md-option value="admin">Admin</md-option>
                        <md-option value="agent">Agent</md-option>
                        <md-option value="junior-agent">Junior Agent</md-option>
                        <md-option value="senior-agent">Senior Agent</md-option>
                      </md-select>
                    </md-field>
                  </div>
                  <div class="md-layout-item md-small-size-100 md-size-50">
                    <md-field>
                      <label>Comission Earned</label>
                      <span class="md-prefix">$</span>
                      <md-input value="1000"></md-input>
                    </md-field>
                  </div>
                </div>
              </div>
              <div class="md-layout-item md-size-100">
                <div class="add-agent-link d-flex align-center cursor-pointer">
                  <div class="add-icon mr-2">
                    <md-icon>add</md-icon>
                  </div>
                  <span class="text-success">Add agent</span>
                </div>
              </div>
            </div>
          </md-content>
          <div class="d-block w-100 text-center">
            <md-button type="submit" class="md-primary button-primary-custom mb-2 mt-3 px-4">
              <span>Save changes</span>
            </md-button>
            <br />
            <div class="d-block text-center">
              <md-button
                :md-ripple="false"
                @click="editAgents = false"
                class="cancel-link lighter-description mt-2 mb-3 text-capitalize"
              >Cancel</md-button>
            </div>
          </div>
        </form>
      </div>
    </md-dialog>
    <!-- transaction step edit -->
    <md-dialog :md-active.sync="transactionStepsEdit" class="modal-medium">
      <md-dialog-title class="modal-title d-block py-3 px-4 bg-light position-relative">
        <h4 class="my-0 md-headline text-left text-primary">Edit transaction step</h4>
        <md-button @click="transactionStepsEdit = false" class="md-icon-button position-absolute">
          <md-icon class="text-primary">close</md-icon>
        </md-button>
      </md-dialog-title>
      <div class="md-dialog-content p-0">
        <md-content md-dynamic-height class="px-4">
          <div class="md-layout">
            <div class="md-layout-item md-small-size-100 md-size-100">
              <md-field slot="inputs">
                <label>Step</label>
                <md-select name="role" id="role">
                  <md-option value="step1">Step 1</md-option>
                  <md-option value="step2">Step 2</md-option>
                  <md-option value="step3">Step 3</md-option>
                  <md-option value="step4">Step 4</md-option>
                </md-select>
              </md-field>
            </div>
            <div class="md-layout-item md-small-size-100 md-size-50">
              <md-field slot="inputs">
                <label>Title</label>
                <md-input></md-input>
              </md-field>
            </div>
            <div class="md-layout-item md-small-size-100 md-size-50">
              <md-field slot="inputs">
                <label>Est. due date</label>
                <md-input></md-input>
              </md-field>
            </div>
            <div class="md-layout-item md-small-size-100 md-size-50">
              <md-field slot="inputs">
                <label>Select a video integration</label>
                <md-select name="role" id="role">
                  <md-option value="admin">Admin</md-option>
                  <md-option value="agent">Agent</md-option>
                  <md-option value="junior-agent">Junior Agent</md-option>
                  <md-option value="senior-agent">Senior Agent</md-option>
                </md-select>
              </md-field>
            </div>
            <div class="md-layout-item md-small-size-100 md-size-50">
              <md-field slot="inputs">
                <label>URL (Vimeo, Youtube, ...)</label>
                <md-input></md-input>
              </md-field>
            </div>
            <div class="md-layout-item md-small-size-100 md-size-100">
              <md-field slot="inputs">
                <label>Description</label>
                <md-textarea
                  class="pl-0"
                  value="At vero eos et accusamus et iusto odio dignissimos ducimus qui blanditiis praesentium voluptatum deleniti atque corrupti quos dolores et quas molestias."
                ></md-textarea>
              </md-field>
            </div>
          </div>
        </md-content>
        <md-dialog-actions class="flex-wrap justify-content-center">
          <md-button
            class="md-primary w-100 button-primary-custom mb-1 mt-3"
            @click="transactionStepsEdit = false"
          >Add</md-button>
          <div class="d-block w-100 text-center">
            <md-button
              :md-ripple="false"
              @click="transactionStepsEdit = false"
              class="cancel-link lighter-description mt-2 mb-3 text-capitalize"
            >Cancel</md-button>
          </div>
        </md-dialog-actions>
      </div>
    </md-dialog>
    <!-- Import Template -->
    <md-dialog :md-active.sync="importTemplate" class="modal-medium">
      <md-dialog-title class="modal-title d-block py-3 px-4 bg-light position-relative">
        <h4 class="my-0 md-headline text-left text-primary">Import template</h4>
        <md-button @click="importTemplate = false" class="md-icon-button position-absolute">
          <md-icon class="text-primary">close</md-icon>
        </md-button>
      </md-dialog-title>
      <div class="md-dialog-content p-0">
        <md-content md-dynamic-height class="px-4">
          <p class="text-primary description text-center">Select a template to import below</p>
          <div class="md-layout">
            <div class="md-layout-item md-small-size-100 md-size-100">
              <md-field slot="inputs">
                <label>Select Template</label>
                <md-select name="role" id="role">
                  <md-option value="step1">Step 1</md-option>
                  <md-option value="step2">Step 2</md-option>
                  <md-option value="step3">Step 3</md-option>
                  <md-option value="step4">Step 4</md-option>
                </md-select>
              </md-field>
            </div>
          </div>
        </md-content>
        <md-dialog-actions class="flex-wrap justify-content-center">
          <md-button
            class="md-primary w-100 button-primary-custom mb-1 mt-3"
            @click="importTemplate = false"
          >Import</md-button>
          <div class="d-block w-100 text-center">
            <md-button
              :md-ripple="false"
              @click="importTemplate = false"
              class="cancel-link lighter-description mt-2 mb-3 text-capitalize"
            >Cancel</md-button>
          </div>
        </md-dialog-actions>
      </div>
    </md-dialog>
    <!-- Enable Confirmation -->
    <md-dialog :md-active.sync="enableConfirmation" class="modal-medium">
      <md-dialog-title class="modal-title d-block py-3 px-4 bg-light position-relative">
        <h4 class="my-0 md-headline text-left text-primary">Complete step</h4>
        <md-button @click="enableConfirmation = false" class="md-icon-button position-absolute">
          <md-icon class="text-primary">close</md-icon>
        </md-button>
      </md-dialog-title>
      <div class="md-dialog-content p-0">
        <md-content md-dynamic-height class="px-4">
          <p class="text-primary description text-center mt-4 mb-3">
            You have selected to complete
            <span class="brand-primary">Sample Step</span>. All users will receive an email.
          </p>
        </md-content>
        <md-dialog-actions class="flex-wrap justify-content-center">
          <md-button
            class="md-primary w-100 button-primary-custom mb-1 mt-3 position-relative check-icon-button"
            @click="enableConfirmation = false"
          >
            <md-icon class="position-absolute">check</md-icon>Complete step
          </md-button>
          <div class="d-block w-100 text-center">
            <md-button
              :md-ripple="false"
              @click="enableConfirmation = false"
              class="cancel-link lighter-description mt-2 mb-3 text-capitalize"
            >Cancel</md-button>
          </div>
        </md-dialog-actions>
      </div>
    </md-dialog>
    <!-- add transaction user -->
    <md-dialog :md-active.sync="addTransactionUser" class="modal-medium">
      <md-dialog-title class="modal-title d-block py-3 px-4 bg-light position-relative">
        <h4 class="my-0 md-headline text-left text-primary">Add transaction user</h4>
        <md-button @click="addTransactionUser = false" class="md-icon-button position-absolute">
          <md-icon class="text-primary">close</md-icon>
        </md-button>
      </md-dialog-title>
      <div class="md-dialog-content p-0">
        <form>
          <md-content md-dynamic-height class="px-4">
            <div class="md-layout">
              <div class="md-layout-item md-small-size-100 md-size-50">
                <md-field>
                  <label>First name</label>
                  <md-input></md-input>
                </md-field>
              </div>
              <div class="md-layout-item md-small-size-100 md-size-50">
                <md-field>
                  <label>Last name</label>
                  <md-input></md-input>
                </md-field>
              </div>
              <div class="md-layout-item md-small-size-100 md-size-50">
                <md-field>
                  <label>Email address</label>
                  <md-input></md-input>
                </md-field>
              </div>
              <div class="md-layout-item md-small-size-100 md-size-50">
                <md-field>
                  <label>Phone</label>
                  <md-input></md-input>
                </md-field>
              </div>
              <div class="md-layout-item md-small-size-100 md-size-50">
                <md-field>
                  <label>Access code</label>
                  <md-input></md-input>
                </md-field>
              </div>
            </div>
            <div class="md-layout">
              <div class="md-layout-item md-size-45 md-medium-size-45 md-small-size-100">
                <h4 class="md-title text-left text-primary">All steps</h4>
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
                <h4 class="md-title text-left text-primary">Assigned steps</h4>
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
            <div class="md-layout mt-3">
              <div class="md-layout-item md-size-100">
                <div class="d-flex justify-content-between align-center">
                  <md-checkbox>
                    <span class="text-primary">Check this box to send an email to user</span>
                  </md-checkbox>
                </div>
              </div>
            </div>
          </md-content>
          <md-dialog-actions class="flex-wrap justify-content-center">
            <md-button
              class="md-primary w-100 button-primary-custom mb-1 mt-3"
              @click="addTransactionUser = false"
            >Add</md-button>
            <div class="d-block w-100 text-center">
              <md-button
                :md-ripple="false"
                @click="addTransactionUser = false"
                class="cancel-link lighter-description mt-2 mb-3 text-capitalize"
              >Cancel</md-button>
            </div>
          </md-dialog-actions>
        </form>
      </div>
    </md-dialog>
    <!-- Close transaction -->
    <!-- add transaction user -->
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
     <md-datepicker v-model="selectedDate">
      <label>Select date</label>
    </md-datepicker>
    <!-- <draggable v-model="myArray" group="people" @start="onStart" @end="onEnd">
        <div v-for="element in myArray" :key="element.id">{{element.name}}</div>
    </draggable>-->
  </div>

</template>

<script>
import draggable from "vuedraggable";
import LoadingButtonLoader from "../components/LoadingButtonLoader";
import LoadingBar from "../components/LoadingBar";
export default {
  name: "UiKit",
  props: ["app"],
  components: {
    LoadingButtonLoader,
    draggable,
    LoadingBar
  },
  data() {
    return {
      selectedDate: null,
      isOpen: true,
      closeTransaction: false,
      addTransactionUser: false,
      enableConfirmation: false,
      transactionStepsEdit: false,
      importTemplate: false,
      showStep: false,
      addTemplate: false,
      publicDetails: false,
      editDetails: false,
      overviewEdit: false,
      addAgents: false,
      deleteAgent: false,
      editAgents: false,
      radio: false,
      transactionList: [
        {
          transaction: "ad ad",
          addressStreet: "618 Blue Road",
          addressCity: "Los Angeles",
          firstName: "John",
          lastName: "Doll",
          type: "Buyer",
          created: "Sep 30, 2019"
        }
      ],
      agentList: [
        {
          firstName: "Alex",
          lastName: "Lopez",
          email: "alex@lopez.com",
          phone: "+1 234 56789",
          title: "Co-ordinator",
          commision: "$1000"
        }
      ],
      userList: [
        {
          firstName: "Alex",
          lastName: "Lopez",
          email: "alex@lopez.com",
          phone: "+1 234 56789",
          role: "Buyer",
          access: "1235A"
        }
      ],
      vendorList: [
        {
          firstName: "Alex",
          lastName: "Lopez",
          email: "alex@lopez.com",
          phone: "+1 234 56789",
          company: "Homesphere"
        }
      ],
      users: [
        {
          name: "Dakota Rice",
          emailaddress: "$36,738",
          phonenumber: "1234567897894022222321",
          role: "Oud-Turnhout",
          title: "CEO",
          access: "Normal"
        },
        {
          name: "Dakota Rice",
          emailaddress: "$36,738",
          phonenumber: "1234567897894022222321",
          role: "Oud-Turnhout",
          title: "CEO",
          access: "Normal"
        },
        {
          name: "Dakota Rice",
          emailaddress: "$36,738",
          phonenumber: "1234567897894022222321",
          role: "Oud-Turnhout",
          title: "CEO",
          access: "Normal"
        },
        {
          name: "Dakota Rice",
          emailaddress: "$36,738",
          phonenumber: "1234567897894022222321",
          role: "Oud-Turnhout",
          title: "CEO",
          access: "Normal"
        },
        {
          name: "Dakota Rice",
          emailaddress: "$36,738",
          phonenumber: "1234567897894022222321",
          role: "Oud-Turnhout",
          title: "CEO",
          access: "Normal"
        },
        {
          name: "Dakota Rice",
          emailaddress: "$36,738",
          phonenumber: "1234567897894022222321",
          role: "Oud-Turnhout",
          title: "CEO",
          access: "Normal"
        },
        {
          name: "Dakota Rice",
          emailaddress: "$36,738",
          phonenumber: "1234567897894022222321",
          role: "Oud-Turnhout",
          title: "CEO",
          access: "Normal"
        }
      ],
      activetemplates: [
        {
          templatename: "Template A",
          created: "Aug 30, 2019"
        },
        {
          templatename: "Template B",
          created: "Aug 03, 2019"
        },
        {
          templatename: "Template c",
          created: "Aug 03, 2019"
        }
      ],
      loading: true,
      myArray: [
        {
          name: "based",
          order: 6,
          fixed: false
        },
        {
          name: "draggable",
          order: 2,
          fixed: false
        },
        {
          name: "component",
          order: 3,
          fixed: false
        },
        {
          name: "for",
          order: 4,
          fixed: false
        },
        {
          name: "vue.draggable",
          order: 1,
          fixed: false
        },
        {
          name: "vue.js 2.0",
          order: 5,
          fixed: false
        },
        {
          name: "on",
          order: 7,
          fixed: false
        },
        {
          name: "Sortablejs",
          order: 8,
          fixed: false
        }
      ]
    };
  },
  methods: {
    handleChange() {
      console.log("changed");
    },
    inputChanged(value) {
      this.activeNames = value;
    },
    getComponentData() {
      return {
        on: {
          change: this.handleChange,
          input: this.inputChanged
        },
        attrs: {
          wrap: true
        },
        props: {
          value: this.activeNames
        }
      };
    },
    // onMove({
    //     relatedContext,
    //     draggedContext
    // }) {
    //     const relatedElement = relatedContext.element;
    //     const draggedElement = draggedContext.element;

    //     console.log(draggedElement);

    //     console.log(relatedElement);
    //     // return (
    //     //     (!relatedElement || !relatedElement.fixed) && !draggedElement.fixed
    //     // );
    // },
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

      console.log(this.myArray);
      console.log(this.move(this.myArray, 0, 2));
    },
    toggleAccordion: function() {
      //   console.log(this.isopen);
      this.isOpen = !this.isOpen;
    }
  },
  computed: {
    accordionClasses: function() {
      return {
        "is-closed": !this.isOpen,
        "is-primary": this.isOpen,
        "is-dark": !this.isOpen
      };
    }
  }
};
</script>

<style scoped lang="scss">
section {
  background-color: #fff;
  padding: 2rem;
  border-radius: 6px;
  box-shadow: 0 0 25px rgba(53, 52, 167, 0.05),
    0 6px 25px 0 rgba(53, 52, 167, 0.08), 0 0 10px 0 rgba(53, 52, 167, 0.05);
}
.layout-1 {
  .md-layout-item {
    height: 40px;

    &:nth-child(1) {
      background: #e0e0e0;
    }

    &:nth-child(2) {
      background: #bdbdbd;
    }

    &:nth-child(3) {
      background: #9e9e9e;
    }
  }
}
.layout-2 {
  .md-layout-item {
    height: 40px;

    &:after {
      width: 100%;
      height: 100%;
      display: block;
      background: #ef9a9a;
      content: " ";
    }
  }
}
.layout-3 {
  .md-layout-item {
    height: 72px;

    &:after {
      width: 100%;
      height: 100%;
      display: block;
      content: " ";
    }

    &.md-layout {
      &:after {
        transform: translateY(-100%);
        background: #fff59d;
      }

      .md-layout-item:after {
        height: 40px;
        margin-top: 16px;
        position: relative;
        z-index: 1;
        background: #f48fb1;
      }
    }
  }
}
</style>
