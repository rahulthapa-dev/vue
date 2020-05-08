<template>
<div class="md-layout flex-md-column-reverse">
    <md-dialog-confirm class="modal-small" :md-active.sync="dialogActive" md-title="Confirmation?" md-content="Are you sure want to delete?." md-confirm-text="Agree" md-cancel-text="Disagree" @md-cancel="onCancel" @md-confirm="onConfirm" />
    <md-dialog :md-active.sync="addTemplate" class="modal-medium">
        <md-dialog-title class="modal-title d-block py-3 px-4 bg-light position-relative">
            <h4 class="my-0 md-headline text-left text-primary">Add Template</h4>
            <md-button @click="closeTemplate" class="md-icon-button position-absolute">
                <md-icon class="text-primary">close</md-icon>
            </md-button>
        </md-dialog-title>
        <div class="md-dialog-content p-0">
            <form @submit.prevent="saveTemplate">
                <md-content md-dynamic-height class="px-4 mb-4">
                    <div class="md-layout">
                        <div class="md-layout-item md-small-size-100 md-size-100">
                            <md-field :class="app.getValidationClass($v,'template_name')">
                                <label>Template name</label>

                                <md-input v-model="template_name"></md-input>
                                <span v-if="!$v.template_name.required" class="md-error">Template name is required</span>
                            </md-field>
                        </div>
                    </div>
                    <div class="md-layout">
                        <div class="md-layout-item md-size-45 md-medium-size-45 md-small-size-100">
                            <h4 class="md-title text-left text-primary">Available steps</h4>
                            <md-list class="list-add-template">
                                <md-list-item class="mb-2" v-for="item in checkedSteps" :key="item.id">
                                    <input type="checkbox" :id="item.id" :value="item.id" v-model="item.checked" />
                                    <label :for="item.id">
                                        <div class="position-relative flex-wrap justify-content-start d-flex align-center">
                                            <div class="md-avatar position-absolute">AB</div>
                                            <p class="md-title text-primary">{{ item.steps_title}}</p>
                                        </div>
                                        <i class="fas fa-angle-double-right description pr-3"></i>
                                    </label>
                                </md-list-item>
                            </md-list>
                        </div>
                        <div class="d-flex align-center justify-content-center mx-auto outer-swap-wrapper">
                            <div class="outer-swap">
                                <div class="inner-swap" @click="dragSelection">
                                    <i class="fas fa-angle-double-left text-primary"></i>
                                    <i class="fas fa-angle-double-right text-primary"></i>
                                </div>
                            </div>
                        </div>
                        <div class="md-layout-item md-size-45 md-medium-size-45 md-small-size-100">
                            <h4 class="md-title text-left text-primary">Assigned steps</h4>
                            <md-list class="list-add-template assigned-list">
                                <draggable v-model="selectedSteps" group="people" @start="onStart" @end="onEnd" >
                                    <md-list-item class="mb-2" :data-id="index" v-for="(item,index) in selectedSteps" :key="item.id">
                                        <input type="checkbox" :id="item.id" :value="item.id" v-model="item.checked" />
                                        <label :for="item.id">
                                            <div class="position-relative flex-wrap justify-content-start d-flex align-center">
                                                <div class="md-avatar position-absolute">AB</div>
                                                <p class="md-title text-primary">{{ item.steps_title}}</p>
                                            </div>
                                            <i class="fas fa-angle-double-left description pr-3"></i>
                                        </label>
                                    </md-list-item>
                                </draggable>
                            </md-list>
                        </div>
                        <!-- <span>Selectd steps: {{ selectedSteps }}</span> -->
                    </div>
                </md-content>
                <md-dialog-actions class="flex-wrap justify-content-center">
                    <md-button type="submit" class="md-primary w-100 button-primary-custom mb-2 mt-3">
                        <LoadingButtonLoader :typeEnable="app.checkType('socialLoader')" v-bind:enable="app.loading" class="mr-1" /> <span v-if="!editStatus">Add</span>
                        <span v-else>Update</span></md-button>
                    <div class="d-block w-100 text-center">
                        <md-button :md-ripple="false" @click="addTemplate = false" class="cancel-link lighter-description mt-2 mb-2 text-capitalize">Cancel</md-button>
                    </div>
                </md-dialog-actions>
            </form>
            <!-- <div id='example-3'>
                <span v-for="item in checkedSteps" :key="item.id">
                    <input type="checkbox" :id="item.id" :value="item.id" v-model="item.checked">
                    <label for="jack">{{ item.steps_title}}</label>
                </span>
                <br>
                <span>Checked items: {{ checkedSteps }}</span>
            </div>
            <div class="d-flex align-center justify-content-center mx-auto" @click="dragSelection">
                <div class="outer-swap">
                    <div class="inner-swap">
                        <i class="fas fa-angle-double-left text-primary"></i>
                        <i class="fas fa-angle-double-right text-primary"></i>
                    </div>
                </div>
            </div>
            <div id='example-3'>
                <span v-for="item in selectedSteps" :key="item.id">
                    <input type="checkbox" :id="item.id" :value="item.id" v-model="item.checked">
                    <label for="jack">{{ item.steps_title}}</label>
                </span>
                <br>
                <span>Selectd steps: {{ selectedSteps }}</span>
            </div> -->
            <!-- <md-content md-dynamic-height class="px-4 mb-4">
                <div class="md-layout">
                    <div class="md-layout-item md-small-size-100 md-size-100">
                        <md-field>
                            <label>Template name</label>
                            <md-input></md-input>
                        </md-field>
                    </div>
                </div>
                <div class="md-layout">
                    <md-list class="list-add-template">
                        <div v-for="(list, i) in lists" :key="i">
                            <drop class="drop list" @drop="handleDrop(list, ...arguments)">
                                <drag v-for="(item,index) in list" class="drag" :key="item" :transfer-data="{ item: item, list: list, example: 'lists' }">
                                    <md-list-item class="mb-2">
                                        <div class="position-relative flex-wrap justify-content-start d-flex align-center">
                                            <div class="md-avatar position-absolute">AB</div>
                                            <p class="md-title text-primary">Step {{index}}</p>
                                        </div>
                                        <i class="fas fa-angle-double-right description pr-3"></i>
                                    </md-list-item>
                                </drag>
                            </drop>
                        </div>
                    </md-list>
                    <div class="d-flex align-center justify-content-center mx-auto">
                        <div class="outer-swap">
                            <div class="inner-swap">
                                <i class="fas fa-angle-double-left text-primary"></i>
                                <i class="fas fa-angle-double-right text-primary"></i>
                            </div>
                        </div>
                    </div>
                </div>
            </md-content> -->
        </div>
    </md-dialog>

    <md-dialog :md-active.sync="showActiveTemplate">
        <!--System Steps  Dialog-->
        <md-dialog-title class="modal-title d-block py-3 px-4 bg-light position-relative">
            <h4 class="my-0 md-headline text-left text-primary">Active Template</h4>
            <md-button @click="showActiveTemplate = false" class="md-icon-button position-absolute">
                <md-icon class="text-primary">close</md-icon>
            </md-button>
        </md-dialog-title>
        <div class="md-dialog-content p-0">
            <md-content md-dynamic-height class="px-4">
                <md-table class="mb-3 system-steps" md-sort="name" md-sort-order="asc">
                    <md-table-row class="table-head">
                        <md-table-head>Template Title ac</md-table-head>
                        <md-table-head>Steps</md-table-head>
                        <md-table-head>Created on</md-table-head>
                        <md-table-head></md-table-head>
                    </md-table-row>
                    <md-table-row slot="md-table-row" v-for="item in activeItems" :key="item.id">
                        <md-table-cell md-label>{{ item.template_name }}</md-table-cell>
                        <md-table-cell md-label="Template Steps" class="steps">
                            <md-list>
                                <md-list-item v-for="step in item.steps_titles.split(',')" :key="step.id">{{ step }}</md-list-item>
                            </md-list>
                        </md-table-cell>
                        <md-table-cell md-label="Created">{{ item.created_at|formatDate }}</md-table-cell>
                        <md-table-cell md-label class="justify-content-end">
                            <md-menu md-direction="bottom-end">
                                <md-button md-menu-trigger class="md-icon-button icon-button-flat">
                                    <md-icon>more_vert</md-icon>
                                </md-button>
                                <md-menu-content class="menu-content-custom">
                                    <md-menu-item @click="editTemplate(item.id,item.template_name,1)">Edit</md-menu-item>
                                    <md-menu-item @click="archiveTemplate(item.id)">Archive</md-menu-item>
                                </md-menu-content>
                            </md-menu>
                        </md-table-cell>
                    </md-table-row>
                </md-table>
                <div class="md-layout">
                    <div class="md-layout-item md-medium-size-50 md-xsmall-size-100 md-size-50 d-flex align-center">
                        <p class="description font-weight-medium" v-if="activeTemplates.length > 1">Displaying {{ activeItems.length }} of {{ activeTemplates.length }} records</p>
                    </div>
                    <div class="md-layout-item md-medium-size-50 md-xsmall-size-100 md-size-50" v-if="activeTemplates.length > 2">
                        <!-- for pagination you can check vue material kit -->
                        <ul class="pagination">
                            <li class="page-item prev-page" v-if="page != 1" v-bind:class="[page == pageNumber ? 'active' : '']">
                                <a @click="page--" class="page-link" aria-label="Previous">
                                    <i class="fas fa-angle-double-left"></i>
                                    <span v-if="withText" class="pr-1">Prev</span>
                                </a>
                            </li>
                            <li class="page-item" v-for="pageNumber in pages.slice(page-1, page+5)" v-bind:key="pageNumber" v-bind:class="[page == pageNumber ? 'active' : '']">
                                <a @click="page = pageNumber" class="page-link">{{ pageNumber }}</a>
                            </li>

                            <li class="page-item page-pre next-page" v-if="page < pages.length" v-bind:class="[page == pageNumber ? 'active' : '']">
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

    <md-dialog :md-active.sync="showSystemTemplate">
        <!--System Steps  Dialog-->
        <md-dialog-title class="modal-title d-block py-3 px-4 bg-light position-relative">
            <h4 class="my-0 md-headline text-left text-primary">Active Company Template</h4>
            <md-button @click="showSystemTemplate = false" class="md-icon-button position-absolute">
                <md-icon class="text-primary">close</md-icon>
            </md-button>
        </md-dialog-title>
        <div class="md-dialog-content p-0">
            <md-content md-dynamic-height class="px-4">
                <md-table class="mb-3 system-steps" md-sort="name" md-sort-order="asc">
                    <md-table-row class="table-head">
                        <md-table-head>Template Title</md-table-head>
                        <md-table-head>Steps</md-table-head>
                        <md-table-head>Created on</md-table-head>
                        <!-- <md-table-head>Created</md-table-head> -->
                        <md-table-head></md-table-head>
                    </md-table-row>
                    <md-table-row slot="md-table-row" v-for="item in systemItems" :key="item.id">
                        <md-table-cell md-label>{{ item.template_name }}</md-table-cell>
                        <md-table-cell md-label="Template Steps" class="steps">
                            <md-list>
                                <md-list-item v-for="step in (item.steps_titles.split(','))" :key="step.id">{{ step }}</md-list-item>
                            </md-list>
                        </md-table-cell>
                        <md-table-cell md-label="Created">{{ item.created_at|formatDate }}</md-table-cell>
                        <md-table-cell md-label class="justify-content-end">
                            <md-menu md-direction="bottom-end">
                                <md-button md-menu-trigger class="md-icon-button icon-button-flat">
                                    <md-icon>more_vert</md-icon>
                                </md-button>
                                <md-menu-content class="menu-content-custom">
                                    <md-menu-item @click="editTemplate(item.id,item.template_name,2)">Edit</md-menu-item>
                                    <md-menu-item @click="archiveTemplate(item.id)">Archive</md-menu-item>
                                </md-menu-content>
                            </md-menu>
                        </md-table-cell>
                    </md-table-row>
                </md-table>
                <div class="md-layout">
                    <div class="md-layout-item md-medium-size-50 md-xsmall-size-100 md-size-50 d-flex align-center">
                        <p class="description font-weight-medium" v-if="systemTemplates.length > 1">Displaying {{ systemItems.length }} of {{ systemTemplates.length }} records</p>
                    </div>
                    <div class="md-layout-item md-medium-size-50 md-xsmall-size-100 md-size-50" v-if="systemTemplates.length > 2">
                        <!-- for pagination you can check vue material kit -->
                        <ul class="pagination">
                            <li class="page-item prev-page" v-if="page2 != 1" v-bind:class="[page2 == pageNumber2 ? 'active' : '']">
                                <a @click="page2--" class="page-link" aria-label="Previous">
                                    <i class="fas fa-angle-double-left"></i>
                                    <span v-if="withText" class="pr-1">Prev</span>
                                </a>
                            </li>
                            <li class="page-item" v-for="pageNumber2 in pages.slice(page2-1, page2+5)" v-bind:key="pageNumber2" v-bind:class="[page2 == pageNumber2 ? 'active' : '']">
                                <a @click="page2 = pageNumber2" class="page-link">{{ pageNumber2 }}</a>
                            </li>

                            <li class="page-item page-pre next-page" v-if="page2 < pages2.length" v-bind:class="[page2 == pageNumber2 ? 'active' : '']">
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

    <md-dialog :md-active.sync="showArchivedTemplate">
        <!--System Steps  Dialog-->
        <md-dialog-title class="modal-title d-block py-3 px-4 bg-light position-relative">
            <h4 class="my-0 md-headline text-left text-primary">Active Archive Template</h4>
            <md-button @click="showArchivedTemplate = false" class="md-icon-button position-absolute">
                <md-icon class="text-primary">close</md-icon>
            </md-button>
        </md-dialog-title>
        <div class="md-dialog-content p-0">
            <md-content md-dynamic-height class="px-4">
                <md-table class="mb-3 system-steps" md-sort="name" md-sort-order="asc">
                    <md-table-row class="table-head">
                        <md-table-head>Template Title</md-table-head>
                        <md-table-head>Steps</md-table-head>
                        <md-table-head>Created on</md-table-head>
                        <md-table-head></md-table-head>
                    </md-table-row>
                    <md-table-row slot="md-table-row" v-for="item in archiveItems" :key="item.id">
                        <md-table-cell md-label>{{ item.template_name }}</md-table-cell>
                        <md-table-cell md-label="Template Steps" class="steps">
                            <md-list>
                                <md-list-item v-for="step in (item.steps_titles.split(','))" :key="step.id">{{ step }}</md-list-item>
                            </md-list>
                        </md-table-cell>
                        <md-table-cell md-label="Created">{{ item.created_at|formatDate }}</md-table-cell>
                        <!-- <md-table-cell md-label class="justify-content-end">
                            <md-menu md-direction="bottom-end">
                                <md-button md-menu-trigger class="md-icon-button icon-button-flat">
                                    <md-icon>more_vert</md-icon>
                                </md-button>
                                <md-menu-content class="menu-content-custom">
                                    <md-menu-item @click="editTemplate(item.id,item.template_name,3)">Edit</md-menu-item>
                                    <md-menu-item @click="archiveTemplate(item.id)">Delete</md-menu-item>
                                </md-menu-content>
                            </md-menu>
                        </md-table-cell> -->
                    </md-table-row>
                </md-table>
                <div class="md-layout">
                    <div class="md-layout-item md-medium-size-50 md-xsmall-size-100 md-size-50 d-flex align-center">
                        <p class="description font-weight-medium" v-if="archiveTemplates.length > 1">Displaying {{ archiveItems.length }} of {{ archiveTemplates.length }} records</p>
                    </div>
                    <div class="md-layout-item md-medium-size-50 md-xsmall-size-100 md-size-50" v-if="archiveTemplates.length > 2">
                        <!-- for pagination you can check vue material kit -->
                        <ul class="pagination">
                            <li class="page-item prev-page" v-if="page3 != 1" v-bind:class="[page3 == pageNumber3 ? 'active' : '']">
                                <a @click="page3--" class="page-link" aria-label="Previous">
                                    <i class="fas fa-angle-double-left"></i>
                                    <span v-if="withText" class="pr-1">Prev</span>
                                </a>
                            </li>
                            <li class="page-item" v-for="pageNumber3 in pages3.slice(page3-1, page2+5)" v-bind:key="pageNumber3" v-bind:class="[page3 == pageNumber3 ? 'active' : '']">
                                <a @click="page3 = pageNumber3" class="page-link">{{ pageNumber3 }}</a>
                            </li>

                            <li class="page-item page-pre next-page" v-if="page3 < pages3.length" v-bind:class="[page3 == pageNumber3 ? 'active' : '']">
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
    <div class="md-layout-item md-medium-size-100 md-xsmall-size-100 md-size-100">
        <md-card class="md-card-plain md-card-custom custom-shadow radius-10 mt-0">
            <md-card-header class="px-4 mb-0 mt-0">
                <div class="md-headline">
                    <span class="text-primary">Templates Settings</span>
                    <span class="brand-primary">
                        <md-icon class="brand-primary">keyboard_arrow_right</md-icon>Templates
                    </span>
                </div>
            </md-card-header>
            <md-card-content class="px-4 py-3">
                <div class="md-layout">
                    <div class="md-layout-item d-flex justify-content-between mb-3">
                        <div class="d-flex align-center justify-start">
                            <label for class="mr-3 active-label description d-flex align-center justify-content-center">Active</label>
                            <h3 class="text-primary font-20 my-0">Active Templates</h3>
                        </div>
                        <md-button class="md-primary md-icon-button" @click="openTemplate(1)">
                            <md-icon>add</md-icon>
                        </md-button>
                    </div>
                </div>
                <!-- {{ activeTemplates }} -->
                <md-table class="mb-3 active-template" md-sort="name" md-sort-order="asc">
                    <md-table-row class="table-head">
                        <md-table-head>Template Title</md-table-head>
                        <md-table-head>Steps</md-table-head>
                        <md-table-head>Created on</md-table-head>
                        <md-table-head></md-table-head>
                    </md-table-row>
                    <md-table-row slot="md-table-row" v-for="item in activeTemplates.slice(0,2)" :key="item.id">
                        <md-table-cell md-label>{{ item.template_name }}</md-table-cell>
                        <md-table-cell md-label="Template Steps" class="steps">
                            <!-- {{ item.steps_titles }} -->
                            <md-list>
                                <md-list-item v-for="step in (item.steps_titles.split(','))" :key="step.id">{{ step }}</md-list-item>
                            </md-list>
                        </md-table-cell>
                        <md-table-cell md-label="Created">{{ item.created_at|formatDate }}</md-table-cell>
                        <md-table-cell md-label class="justify-content-end">
                            <md-menu md-direction="bottom-end">
                                <md-button md-menu-trigger class="md-icon-button icon-button-flat">
                                    <md-icon>more_vert</md-icon>
                                </md-button>
                                <md-menu-content class="menu-content-custom">
                                    <md-menu-item @click="editTemplate(item.id,item.template_name,1)">Edit</md-menu-item>
                                    <md-menu-item @click="archiveTemplate(item.id)">Archive</md-menu-item>
                                </md-menu-content>
                            </md-menu>
                        </md-table-cell>
                    </md-table-row>
                </md-table>
                <div class="md-layout">
                    <div class="md-layout-item md-medium-size-50 md-xsmall-size-100 md-size-50 d-flex align-center">
                        <!-- <p class="description font-weight-medium">Displaying 10 of 20 records</p> -->
                        <p class="description font-weight-medium" v-if="activeTemplates.length > 1">Displaying 2 of {{ activeTemplates.length }} records</p>
                    </div>
                    <div class="md-layout-item md-medium-size-50 md-xsmall-size-100 md-size-50 justify-content-end d-flex">
                        <md-button class="load-more-button md-primary" @click="showActiveTemplate=true">
                            <i class="fas fa-sync-alt"></i> View all
                        </md-button>
                    </div>
                </div>
            </md-card-content>
            <md-card-content class="px-4 py-3">
                <div class="md-layout">
                    <div class="md-layout-item d-flex justify-content-between mb-3">
                        <div class="d-flex align-center justify-start">
                            <label for class="mr-3 active-label description d-flex align-center justify-content-center">Active</label>
                            <h3 class="text-primary font-20 my-0">Active Company Templates</h3>
                        </div>
                        <md-button class="md-primary md-icon-button" @click="openTemplate(2)">
                            <md-icon>add</md-icon>
                        </md-button>
                    </div>
                </div>

                <md-table class="mb-3 active-template" md-sort="name" md-sort-order="asc">
                    <md-table-row class="table-head">
                        <md-table-head>Template Title</md-table-head>
                        <md-table-head>Steps</md-table-head>
                        <md-table-head>Created on</md-table-head>
                        <md-table-head></md-table-head>
                    </md-table-row>
                    <md-table-row slot="md-table-row" v-for="item in systemTemplates.slice(0,2)" :key="item.id">
                        <md-table-cell md-label>{{ item.template_name }}</md-table-cell>
                        <md-table-cell md-label="Template Steps" class="steps">
                            <md-list>
                                <md-list-item v-for="step in (item.steps_titles.split(','))" :key="step.id">{{ step }}</md-list-item>
                            </md-list>
                        </md-table-cell>
                        <md-table-cell md-label="Created">{{ item.created_at|formatDate }}</md-table-cell>
                        <md-table-cell md-label class="justify-content-end">
                            <md-menu md-direction="bottom-end">
                                <md-button md-menu-trigger class="md-icon-button icon-button-flat">
                                    <md-icon>more_vert</md-icon>
                                </md-button>
                                <md-menu-content class="menu-content-custom">
                                    <md-menu-item @click="editTemplate(item.id,item.template_name,2)">Edit</md-menu-item>
                                    <md-menu-item @click="archiveTemplate(item.id)"> Archive</md-menu-item>
                                </md-menu-content>
                            </md-menu>
                        </md-table-cell>
                    </md-table-row>
                </md-table>
                <div class="md-layout">
                    <div class="md-layout-item md-medium-size-50 md-xsmall-size-100 md-size-50 d-flex align-center">
                        <!-- <p class="description font-weight-medium">Displaying 10 of 20 records</p> -->
                        <p class="description font-weight-medium" v-if="systemTemplates.length > 1">Displaying 2 of {{ systemTemplates.length }} records</p>
                    </div>
                    <div class="md-layout-item md-medium-size-50 md-xsmall-size-100 md-size-50 justify-content-end d-flex">
                        <md-button class="load-more-button md-primary" @click="showSystemTemplate=true">
                            <i class="fas fa-sync-alt"></i> View all
                        </md-button>
                    </div>
                </div>
            </md-card-content>
            <md-card-content class="px-4 py-3">
                <div class="md-layout">
                    <div class="md-layout-item d-flex justify-content-between mb-3">
                        <div class="d-flex align-center justify-start">
                            <label for class="mr-3 archived-label description d-flex align-center justify-content-center">Archived</label>
                            <h3 class="text-primary font-20 my-0">Templates</h3>
                        </div>
                    </div>
                </div>

                <md-table class="mb-3 active-template" md-sort="name" md-sort-order="asc" v-if="archiveTemplates.length">
                    <md-table-row class="table-head">
                        <md-table-head>Template Title</md-table-head>
                        <md-table-head>Steps</md-table-head>
                        <md-table-head>Created on</md-table-head>
                        <md-table-head></md-table-head>
                    </md-table-row>
                    <md-table-row slot="md-table-row" v-for="item in archiveTemplates.slice(0,2)" :key="item.id">
                        <md-table-cell md-label>{{ item.template_name }}</md-table-cell>
                        <md-table-cell md-label="Template Steps" class="steps">
                            <md-list>
                                <md-list-item v-for="step in (item.steps_titles.split(','))" :key="step.id">{{ step }}</md-list-item>
                            </md-list>
                        </md-table-cell>
                        <md-table-cell md-label="Created">{{ item.created_at|formatDate }}</md-table-cell>
                        <!-- <md-table-cell md-label class="justify-content-end">
                            <md-menu md-direction="bottom-end">
                                <md-button md-menu-trigger class="md-icon-button icon-button-flat">
                                    <md-icon>more_vert</md-icon>
                                </md-button>
                                <md-menu-content class="menu-content-custom">
                                    <md-menu-item @click="editTemplate(item.id)">Edit</md-menu-item>
                                    <md-menu-item @click="deleteTemplate(item.id)"> Delete</md-menu-item>
                                </md-menu-content>
                            </md-menu>
                        </md-table-cell> -->
                    </md-table-row>
                </md-table>
                <div class="md-layout">
                    <div class="md-layout-item md-medium-size-50 md-xsmall-size-100 md-size-50 d-flex align-center">
                        <p class="description font-weight-medium" v-if="archiveTemplates.length > 1">Displaying 2 of {{ archiveTemplates.length }} records</p>
                    </div>
                    <div class="md-layout-item md-medium-size-50 md-xsmall-size-100 md-size-50 justify-content-end d-flex">
                        <md-button class="load-more-button md-primary" @click="showArchivedTemplate=true">
                            <i class="fas fa-sync-alt"></i> View all
                        </md-button>
                    </div>
                </div>
            </md-card-content>
        </md-card>
    </div>
    <!-- <div class="drag-container" v-drag-and-drop:options="options">
        <ul class="drag-list">
            <li class="drag-column" v-for="group in groups" :key="group.id">
                <span class="drag-column-header">
                    <h2>{{ group.name }}</h2>
                </span>
                <vue-draggable-group v-model="group.items" :groups="groups" :data-id="group.id" @change="onGroupsChange">
                    <ul class="drag-inner-list" :data-id="group.id">
                        <li class="drag-item" v-for="item in group.items" :key="item.id" :data-id="item.id">
                            <div class="drag-item-text">{{ item.name }}</div>
                        </li>
                    </ul>
                </vue-draggable-group>
            </li>
        </ul>
    </div> -->
    <!-- <draggable v-model="myArray" group="people" @start="onStart" @end="onEnd">
        <div v-for="element in myArray" :key="element.id">{{element.name}}</div>
    </draggable> -->
</div>
</template>

<script>
import {
    HTTP
} from '../../httpCommon';
import draggable from 'vuedraggable';
import LoadingBar from "../../components/LoadingBar";
import LoadingButtonLoader from "../../components/LoadingButtonLoader";
import {
    required,
    email,
    minLength,
    maxLength,
    sameAs
} from "vuelidate/lib/validators";
export default {
    name: "Templates",
    props: ["app"],
    components: {
        draggable,
        LoadingBar,
        LoadingButtonLoader
    },
    data: function () {
        return {
            activetemplates: [{
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

            lists: [
                ['A', 'B', 'C'],
                ['D', 'E', 'F'],
            ],
            addTemplate: false,
            activeSteps: [{
                    "id": 1,
                    "steps_title": "Business",
                    "checked": false
                },
                {
                    "id": 2,
                    "steps_title": "Business 2",
                    "checked": false
                },
            ],
            checkedArray: [],
            checkedSteps: [],
            selectedSteps: [],
            options: {
                dropzoneSelector: 'ul',
                draggableSelector: 'li',
                handlerSelector: null,
                reactivityEnabled: true,
                multipleDropzonesItemsDraggingEnabled: true,
                showDropzoneAreas: true,
                onDrop: function (event) {},
                onDragstart: function (event) {

                },
                onDragenter: function (event) {},
                onDragover: function (event) {},
                onDragend: function (event) {}
            },
            groups: [{
                    id: 1,
                    name: "To Do",
                    items: [{
                            id: 1,
                            name: "Item 1",
                            groupId: 1
                        },
                        {
                            id: 2,
                            name: "Item 2",
                            groupId: 1
                        },
                        {
                            id: 3,
                            name: "Item 3",
                            groupId: 1
                        }
                    ]
                },
                {
                    id: 2,
                    name: "In Progress",
                    items: [{
                            id: 4,
                            name: "Item 4",
                            groupId: 2
                        },
                        {
                            id: 5,
                            name: "Item 5",
                            groupId: 2
                        },
                        {
                            id: 6,
                            name: "Item 6",
                            groupId: 2
                        }
                    ]
                },
                {
                    id: 3,
                    name: "Done",
                    items: [{
                            id: 7,
                            name: "Item 7",
                            groupId: 3
                        },
                        {
                            id: 8,
                            name: "Item 8",
                            groupId: 3
                        },
                        {
                            id: 9,
                            name: "Item 9",
                            groupId: 3
                        },
                        {
                            id: 10,
                            name: "Item 10",
                            groupId: 3
                        }
                    ]
                }
            ],
            editStatus: false,
            template_name: "",
            templateArray: [],
            activeTemplates: [],
            systemTemplates: [],
            archiveTemplates: [],
            items: "",
            totalItems: "",
            deleteTemplateId: "",
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
            showActiveTemplate: false,
            showSystemTemplate: false,
            showArchivedTemplate: false,
            withText: Boolean,
            dialogActive: false,
            editStatus: false,
            template_type: ""
        };
    },
    mounted() {
        this.init();
        this.getSteps();
    },
    validations: {
        template_name: {
            required
        }
    },
    computed: {
        activeItems() {
            console.log("active item change");
            return this.paginate(this.activeTemplates);
        },
        systemItems() {
            console.log("system item change");
            return this.paginate2(this.systemTemplates);
        },
        archiveItems() {
            console.log("archive item change");
            return this.paginate3(this.archiveTemplates);
        }
    },
    watch: {},
    methods: {
        init() {
            HTTP
                .get("api/templates/getTemplates")
                .then(response => {
                    if (response.data.Status) {
                        this.activeTemplates = response.data.ActiveTemplates;
                        this.systemTemplates = response.data.SystemTemplates;
                        this.archiveTemplates = response.data.ArchiveTemplates;
                        let activeTemplates = this.activeTemplates;

                        this.setPages(this.activeTemplates.length);
                        this.setPages2(this.systemTemplates.length);
                        this.setPages3(this.archiveTemplates.length);
                    }
                })
                .catch(error => {

                });
        },
        openTemplate(option) {
            this.editStatus = false;
            this.addTemplate = true;
            this.getSteps();
            this.selectedSteps = [];
            this.template_name = "";
            this.template_type = option;
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
        getSteps() {
            HTTP
                .get("api/steps/getTemplateSteps")
                .then(response => {
                    if (response.data.Status) {
                        this.activeSteps = response.data.Steps;
                        let activeSteps = this.activeSteps;

                        this.checkedSteps = this.activeSteps.map((item, i) => {
                            return {
                                "id": item.id,
                                "steps_title": item.steps_title,
                                "checked": false,
                                "order": i
                            }
                        })
                    }
                })
                .catch(error => {

                });
        },
        archiveTemplate(id) {
            this.app.loading = true;
            const data = {
                template_id: id
            };
            HTTP.post("api/steps/archiveTemplate", data)
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
                .catch(error => {});
        },
        deleteTemplate(id) {
            this.dialogActive = true;
            this.deleteTemplateId = id;
        },
        onConfirm() {
            this.app.loading = true;

            if (this.deleteTemplateId) {
                HTTP.delete("api/templates/deleteTemplate", {
                        params: {
                            deleteTemplateId: this.deleteTemplateId
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
                    .catch(error => {});
            }
        },
        onCancel() {},
        dragSelection() {
            // console.log(this.checkedNames);
            // console.log(this.activeSteps);

            //For Checked selection
            let removeCheckedArray = this.checkedSteps.map((item, i) => {
                if (item.checked) {
                    console.log(i);
                    this.selectedSteps.push({
                        "id": item.id,
                        "steps_title": item.steps_title,
                        "checked": false,
                        "order": i
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
                        "id": item.id,
                        "steps_title": item.steps_title,
                        "checked": false,
                        "order": i
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
        handleDrop(toList, data) {
            const fromList = data.list;
            if (fromList) {
                toList.push(data.item);
                fromList.splice(fromList.indexOf(data.item), 1);
                toList.sort((a, b) => a > b);
            }
        },
        add() {
            console.log("teasd adsadsadas");
        },
        added() {
            console.log("added");
        },
        removed() {
            console.log("removed");
        },
        reordered($el) {
            console.log($el);
            console.log("reordered");
        },
        onGroupsChange(e) {
            console.log({
                e
            });
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
                while ((k--) + 1) {
                    arr.push(undefined);
                }
            }
            arr.splice(new_index, 0, arr.splice(old_index, 1)[0]);
            return arr;
        },
        onEnd: function ( /**Event*/ evt) {
            var itemEl = evt.item; // dragged HTMLElement
            evt.to; // target list
            evt.from; // previous list
            evt.oldIndex; // element's old index within old parent
            evt.newIndex; // element's new index within new parent
            evt.oldDraggableIndex; // element's old index within old parent, only counting draggable elements
            evt.newDraggableIndex; // element's new index within new parent, only counting draggable elements
            evt.clone // the clone element
            evt.pullMode; // when item is in another sortable: `"clone"` if cloning, `true` if moving

            //debugger;
            console.log(evt.oldIndex);

            console.log(evt.newIndex);

            // console.log(this.myArray);
            // console.log(this.move(this.myArray, 0, 2));
        },
        editTemplate(id, template_name, type) {
            this.showActiveTemplate = false;
            this.showSystemTemplate = false;
            this.showArchivedTemplate = false;
            this.editStatus = true;
            this.template_id = id;
            this.template_name = template_name;
            if (type == 1) {
                // this.getSteps();
                let index = this.activeTemplates.map((item) => {
                    return item.id;
                }).indexOf(id);
                if (index != -1) {
                    // console.log(this.activeTemplates[index]);
                    let selected_array = JSON.parse(this.activeTemplates[index].steps_lists);
                    let selected_titles_array = this.activeTemplates[index].steps_titles.split(',');
                    for (let j = 0; j < selected_array.length; j++) {
                        selected_array[j].steps_title = selected_titles_array[j];
                    }
                    this.selectedSteps = selected_array;
                    // console.log(this.selectedSteps);
                    this.addTemplate = true;
                    for (var i = this.checkedSteps.length - 1; i >= 0; i--) {
                        let index = this.selectedSteps.map((item) => {
                            return item.id;
                        }).indexOf(this.checkedSteps[i].id);

                        if (index != -1) {
                            this.checkedSteps.splice(i, 1); //Remove from checkedSteps
                        }
                    }
                }
            }

            if (type == 2) {
                let index = this.systemTemplates.map((item) => {
                    return item.id;
                }).indexOf(id);
                if (index != -1) {

                    let selected_array = JSON.parse(this.systemTemplates[index].steps_lists);
                    let selected_titles_array = this.systemTemplates[index].steps_titles.split(',');
                    for (let j = 0; j < selected_array.length; j++) {
                        selected_array[j].steps_title = selected_titles_array[j];
                    }
                    this.selectedSteps = selected_array;
                    // console.log(this.selectedSteps);
                    // console.log(this.activeTemplates[index]);
                    // this.selectedSteps = JSON.parse(this.systemTemplates[index].steps_lists);
                    this.addTemplate = true;
                    for (var i = this.checkedSteps.length - 1; i >= 0; i--) {
                        let index = this.selectedSteps.map((item) => {
                            return item.id;
                        }).indexOf(this.checkedSteps[i].id);

                        if (index != -1) {
                            this.checkedSteps.splice(i, 1); //Remove from checkedSteps
                        }
                    }
                }
            }

        },
        closeTemplate() {
            this.getSteps();
            this.selectedSteps = [];
            this.addTemplate = false;
        },
        saveTemplate() {
            this.$v.$touch();
            if (this.$v.$error) return;
            this.app.loading = true;

            console.log(this.template_name);
            console.log(this.selectedSteps);
            console.log(this.templateArray);
            // console.log("saved template----");

            const data = {
                template_id: this.template_id,
                template_name: this.template_name,
                steps_lists: this.selectedSteps,
                template_type: this.template_type
            };

            if (this.editStatus) {
                HTTP.put("api/templates/updateTemplate", data)
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
                            this.addTemplate = false;
                        }
                    })
                    .catch(error => {
                        this.app.loading = false;
                    });
            } else {
                HTTP.post("api/templates/addTemplate", data)
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
                            this.addTemplate = false;
                        }
                    })
                    .catch(error => {
                        this.app.loading = false;
                    });
            }

        }
    },
};
</script>

<style scoped>

</style>
