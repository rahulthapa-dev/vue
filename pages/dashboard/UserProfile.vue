<template>
<div class="content">
    <md-dialog-confirm :md-active.sync="dialogActive" md-title="Confirmation?" md-content="Are you sure want to delete?." md-confirm-text="Agree" md-cancel-text="Disagree" @md-cancel="onCancel" @md-confirm="onConfirm" />

    <!-- <md-button class="md-primary md-raised" @click="active = true">Confirm</md-button> -->
    <md-dialog :md-active.sync="showDialog">

        <md-dialog-title class="modal-title d-block py-3 px-4 bg-light">
            <h4 class="my-0 md-headline text-left text-primary">Update personal information</h4>
        </md-dialog-title>
        <div class="md-dialog-content">
            <form @submit.prevent="updateProfile">
                <md-content md-dynamic-height class="px-4">
                    <div class="md-layout">
                        <div class="md-layout-item md-small-size-100 md-size-50">
                            <md-field slot="inputs" :class="app.getValidationClass($v.userForm,'first_name')">
                                <label>First name</label>
                                <md-input v-model="userForm.first_name"></md-input>
                                <span v-if="!$v.userForm.first_name.required" class="md-error">Firstname is required</span>
                            </md-field>
                        </div>
                        <div class="md-layout-item md-small-size-100 md-size-50">
                            <md-field slot="inputs" :class="app.getValidationClass($v.userForm,'last_name')">
                                <label>Last name</label>
                                <md-input v-model="userForm.last_name"></md-input>
                                <span v-if="!$v.userForm.last_name.required" class="md-error">Lastname is required</span>
                            </md-field>
                        </div>
                        <div class="md-layout-item md-small-size-100 md-size-50">
                            <md-field slot="inputs" :class="app.getValidationClass($v.userForm,'email')">
                                <label>Email</label>
                                <md-input v-model="userForm.email" :disabled="true"></md-input>
                                <span v-if="!$v.userForm.email.required" class="md-error">Email is required</span>
                                <span v-if="!$v.userForm.email.email" class="md-error">Email is Invalid</span>
                            </md-field>
                        </div>
                        <div class="md-layout-item md-small-size-100 md-size-50">
                            <md-field slot="inputs" :class="app.getValidationClass($v.userForm,'phone')" class="multi-error">
                                <label>Phone no.</label>
                                <md-input v-model="userForm.phone"></md-input>
                                <span v-if="!$v.userForm.phone.required" class="md-error">Phone no is required</span>
                                <span v-if="!$v.userForm.phone.phoneValid" class="md-error">Phone no is Invalid</span>
                                <span v-if="!$v.userForm.phone.minLength" class="md-error">minLength is 10</span>
                                <span v-if="!$v.userForm.phone.maxLength" class="md-error">maxLength is 13</span>
                            </md-field>
                        </div>
                        <div class="md-layout-item md-small-size-100 md-size-50">
                            <md-field slot="inputs">
                                <label>License no.</label>
                                <md-input v-model="userForm.licence_number"></md-input>
                            </md-field>
                        </div>
                        <div class="md-layout-item md-small-size-100 md-size-50">
                            <md-field slot="inputs">
                                <md-input v-model="userForm.title"></md-input>
                            </md-field>
                        </div>
                    </div>
                </md-content>
                <md-dialog-actions class="flex-wrap justify-content-center">
                    <md-button type="submit" class="md-primary w-100 button-primary-custom mb-3 mt-3">
                        <LoadingButtonLoader :typeEnable="app.checkType('simpleLoader')" v-bind:enable="app.loading" class="mr-1" />Update
                    </md-button>

                    <!-- {{$v.userForm}} -->
                    <div class="d-block w-100 text-center">
                        <md-button :md-ripple="false" @click="showDialog = false" class="cancel-link lighter-description mt-2 text-capitalize">Cancel</md-button>
                    </div>
                </md-dialog-actions>
            </form>
        </div>

    </md-dialog>
    <md-dialog :md-active.sync="showSocialLinks" class="social-link-modal">
        <md-dialog-title class="modal-title d-block py-3 px-4 bg-light">
            <h4 class="my-0 md-headline text-left text-primary">Update Social Links</h4>
        </md-dialog-title>
        <div class="md-dialog-content">
            <form @submit.prevent="updateSocialLinks">
                <md-content md-dynamic-height class="px-4">
                    <div class="md-layout">
                        <div class="md-layout-item md-small-size-100 md-size-100">
                            <md-field slot="inputs" :class="app.getValidationClass($v.socialForm,'website')">
                                <label>Website URL</label>
                                <md-input type="text" v-model="socialForm.website"></md-input>
                                <md-icon>language</md-icon>
                                <span v-if="!$v.socialForm.website.urlValid" class="md-error">Url invalid</span>
                            </md-field>
                        </div>
                        <div class="md-layout-item md-small-size-100 md-size-100">
                            <md-field slot="inputs" :class="app.getValidationClass($v.socialForm,'facebook')">
                                <label>Facebook URL</label>
                                <md-input type="text" v-model="socialForm.facebook"></md-input>
                                <i class="fab fa-facebook-f description"></i>
                                <span v-if="!$v.socialForm.facebook.urlValid" class="md-error">Url invalid</span>
                            </md-field>
                        </div>
                        <div class="md-layout-item md-small-size-100 md-size-100">
                            <md-field slot="inputs" :class="app.getValidationClass($v.socialForm,'instagram')">
                                <label>Instagram URL</label>
                                <md-input type="text" v-model="socialForm.instagram"></md-input>
                                <i class="fab fa-instagram description"></i>
                                <span v-if="!$v.socialForm.instagram.urlValid" class="md-error">Url invalid</span>
                            </md-field>
                        </div>
                        <div class="md-layout-item md-small-size-100 md-size-100">
                            <md-field slot="inputs" :class="app.getValidationClass($v.socialForm,'zillow')">
                                <label>zillow URL</label>
                                <md-input type="text" v-model="socialForm.zillow"></md-input>
                                <i class="description">
                                    <svg xmlns="http://www.w3.org/2000/svg" width="24" height="18" viewBox="0 0 28.003 30.762">
                                        <path id="Path_2" data-name="Path 2" d="M7.552,27.348a.231.231,0,0,1-.305-.047L4.635,24.19c-.07-.087-.078-.134.016-.266a49.448,49.448,0,0,1,8.764-9.506c.047-.031.031-.071-.024-.047A115.564,115.564,0,0,0,0,20.234V30.8H28V20.625c-3.815.648-15.245,4.05-20.444,6.723ZM18.934,8.335a.258.258,0,0,1,.289.109c.485.539,2.034,2.431,2.455,2.947a.174.174,0,0,1-.038.25,55.852,55.852,0,0,0-8.545,8.365c-.038.055-.007.055.024.047A97.2,97.2,0,0,1,28,15.629V11.1L14.018.04.008,11.094v4.948A98.363,98.363,0,0,1,18.934,8.335Z" transform="translate(0 -0.04)" fill="#b2b2db" />
                                    </svg>
                                </i>
                                <span v-if="!$v.socialForm.zillow.urlValid" class="md-error">Url invalid</span>
                            </md-field>
                        </div>
                        <div class="md-layout-item md-small-size-100 md-size-100">
                            <md-field slot="inputs" :class="app.getValidationClass($v.socialForm,'youtube')">
                                <label>youtube URL</label>
                                <md-input type="text" v-model="socialForm.youtube"></md-input>
                                <i class="description">
                                    <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 33 33">
                                        <g id="video" transform="translate(1.5 1.5)">
                                            <path id="Path" d="M13.5,7.5,0,14.85V0Z" transform="translate(10.5 7.5)" fill="none" stroke="#b2b2db" stroke-linecap="square" stroke-miterlimit="10" stroke-width="3" />
                                            <circle id="Oval" cx="15" cy="15" r="15" fill="none" stroke="#b2b2db" stroke-linecap="square" stroke-miterlimit="10" stroke-width="3" />
                                        </g>
                                    </svg>
                                </i>
                                <span v-if="!$v.socialForm.youtube.urlValid" class="md-error">Url invalid</span>
                            </md-field>
                        </div>
                    </div>
                </md-content>
                <md-dialog-actions class="flex-wrap justify-content-center">
                    <md-button type="submit" class="md-primary w-100 button-primary-custom mb-0 mt-3">
                        <LoadingButtonLoader :typeEnable="app.checkType('socialLoader')" v-bind:enable="app.loading" class="mr-1" />Update
                    </md-button>

                    <div class="d-block w-100 text-center">
                        <md-button :md-ripple="false" @click="showSocialLinks = false" class="cancel-link lighter-description mt-2 text-capitalize">Cancel</md-button>
                    </div>
                    <!-- {{$v.socialForm}} -->
                </md-dialog-actions>
            </form>
        </div>
    </md-dialog>
    <div class="md-layout">
        <div class="md-layout-item md-size-20 md-medium-size-25 md-small-size-100">
            <h4 class="text-primary mb-3">Profile photo</h4>
            <div class="position-relative profile-section-outer" v-bind:class="classObject">
                <label for="profile-image" class="profile-section">
                    <img :src="image" class="image" />
                    <div class="middle">
                        <md-icon>camera_alt</md-icon>
                        <input type="file" id="profile-image" class="form-control" @change="onImageChange" />
                    </div>
                </label>
                <div class="delete cursor-pointer" @click="deleteProfileImg">
                    <md-icon>delete</md-icon>
                    <span>Remove</span>
                </div>
                <!-- <div v-if="imageLoader" class="loader">
          <LoadingBar /></div>-->
            </div>
            <div class="review-section pt-2 pb-3 d-flex justify-content-between align-center">
                <strong class="text-primary">4.5</strong>
                <span class="px-2">
                    <md-icon class="brand-primary">star</md-icon>
                    <md-icon class="brand-primary">star</md-icon>
                    <md-icon class="brand-primary">star</md-icon>
                    <md-icon class="brand-primary">star</md-icon>
                    <md-icon class="brand-primary">star</md-icon>
                </span>
                <span class="description">(856)</span>
            </div>
        </div>

        <div class="md-layout-item md-size-80 md-medium-size-75 md-small-size-100">
            <div class="md-layout md-gutter">
                <div class="md-layout-item md-size-70 md-medium-size-100 md-small-size-100">
                    <div class="mb-5 d-block w-100">
                        <div class="d-flex justify-content-between align-center">
                            <h4 class="text-primary mt-2 mb-3">Personal information</h4>
                            <a class="brand-primary font-weight-medium mt-2 mb-3 cursor-pointer" @click="editProfile">Edit</a>
                        </div>
                        <div class="custom-shadow bg-white radius-10 px-4 py-4">
                            <div class="md-layout">
                                <div class="md-layout-item md-small-size-100 md-medium-size-33 md-size-33 mb-2">
                                    <p class="description-normal lighter-description">Full name</p>
                                    <span>
                                        <h3 v-if="userForm.first_name" class="md-title font-weight-medium text-primary mt-0 text-ellipsis">{{userForm.first_name}}</h3>
                                        <md-tooltip class="tooltip-basic" md-direction="bottom">{{userInfo.first_name}}</md-tooltip>
                                    </span>
                                </div>
                                <div class="md-layout-item md-small-size-100 md-medium-size-33 md-size-33 mb-2">
                                    <p class="description-normal lighter-description">Role</p>
                                    <span>
                                        <h3 v-if="userInfo.role_name" class="md-title font-weight-medium text-primary mt-0 text-ellipsis">{{userInfo.role_name}}</h3>
                                        <md-tooltip class="tooltip-basic" md-direction="bottom">{{userInfo.role_name}}</md-tooltip>
                                    </span>
                                </div>

                                <div class="md-layout-item md-small-size-100 md-medium-size-33 md-size-33 mb-2">
                                    <p class="description-normal lighter-description">Title</p>
                                    <span>
                                        <h3 v-if="userForm.title" class="md-title font-weight-medium text-primary mt-0 text-ellipsis">{{ userForm.title }}</h3>
                                        <md-tooltip class="tooltip-basic" md-direction="bottom">{{ userForm.title }}</md-tooltip>
                                    </span>
                                </div>

                                <div class="md-layout-item md-small-size-100 md-medium-size-33 md-size-33 mb-2">
                                    <p class="description-normal lighter-description">License Number</p>
                                    <span>
                                        <h3 v-if="userForm.licence_number" class="md-title font-weight-medium text-primary mt-0 text-ellipsis">{{ userForm.licence_number }}</h3>
                                        <md-tooltip class="tooltip-basic" md-direction="bottom">{{ userForm.licence_number }}</md-tooltip>
                                    </span>
                                </div>
                                <div class="md-layout-item md-small-size-100 md-medium-size-33 md-size-33 mb-2">
                                    <p class="description-normal lighter-description">E-mail address</p>
                                    <span>
                                        <h3 v-if="userForm.email" class="md-title fonr-weight-medium text-primary mt-0 text-ellipsis">{{ userForm.email }}</h3>
                                        <md-tooltip class="tooltip-basic" md-direction="bottom">{{ userForm.email }}</md-tooltip>
                                    </span>
                                </div>
                                <div class="md-layout-item md-small-size-100 md-medium-size-33 md-size-33 mb-2">
                                    <p class="description-normal lighter-description">Phone number</p>
                                    <span>
                                        <h3 v-if="userForm.phone" class="md-title fonr-weight-medium text-primary mt-0 text-ellipsis">+{{userForm.phone}}</h3>
                                        <md-tooltip class="tooltip-basic" md-direction="bottom">+{{userForm.phone}}</md-tooltip>
                                    </span>
                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="mb-5 d-block w-100">
                        <div class="d-flex justify-content-between align-center">
                            <h4 class="text-primary mb-3">Social links</h4>
                            <a class="brand-primary font-weight-medium mt-2 mb-3 cursor-pointer" @click="editSocial">Edit</a>
                        </div>
                        <div class="custom-shadow bg-white radius-10 px-4 py-4 d-flex flex-wrap social-icon-outer">
                            <div class="d-inline-block mb-2 px-2 pl-0">
                                <span v-if="socialForm.website">
                                    <a :href="socialForm.website" class="social-icon icon-website d-flex align-center justify-content-center" target="_blank">
                                        <i class="material-icons">language</i>
                                    </a>
                                    <md-tooltip>
                                        <h4 class="text-primary description my-0">Website</h4>
                                        <p v-if="socialForm.website" class="lighter-description description-normal">{{socialForm.website}}</p>
                                    </md-tooltip>
                                </span>
                                <span v-else>
                                    <a class="social-icon icon-website d-flex align-center justify-content-center" target="_blank">
                                        <i class="material-icons">language</i>
                                    </a>
                                    <md-tooltip>
                                        <h4 class="text-primary description my-0">Website</h4>
                                        <p class="lighter-description description-normal"></p>
                                    </md-tooltip>
                                </span>
                            </div>
                            <div class="d-inline-block mb-2 px-2">
                                <span v-if="socialForm.zillow">
                                    <a :href="socialForm.zillow" class="social-icon icon-zillow d-flex align-center justify-content-center" target="_blank">
                                        <svg xmlns="http://www.w3.org/2000/svg" width="26.003" height="28.762" viewBox="0 0 28.003 30.762">
                                            <path id="Path_2" data-name="Path 2" d="M7.552,27.348a.231.231,0,0,1-.305-.047L4.635,24.19c-.07-.087-.078-.134.016-.266a49.448,49.448,0,0,1,8.764-9.506c.047-.031.031-.071-.024-.047A115.564,115.564,0,0,0,0,20.234V30.8H28V20.625c-3.815.648-15.245,4.05-20.444,6.723ZM18.934,8.335a.258.258,0,0,1,.289.109c.485.539,2.034,2.431,2.455,2.947a.174.174,0,0,1-.038.25,55.852,55.852,0,0,0-8.545,8.365c-.038.055-.007.055.024.047A97.2,97.2,0,0,1,28,15.629V11.1L14.018.04.008,11.094v4.948A98.363,98.363,0,0,1,18.934,8.335Z" transform="translate(0 -0.04)" fill="#b2b2db" />
                                        </svg>
                                    </a>
                                    <md-tooltip>
                                        <h4 class="text-primary description my-0">Zillow</h4>
                                        <p v-if="socialForm.zillow" class="lighter-description description-normal">{{socialForm.zillow}}</p>
                                    </md-tooltip>
                                </span>
                                <span v-else>
                                    <a class="social-icon icon-zillow d-flex align-center justify-content-center" target="_blank">
                                        <svg xmlns="http://www.w3.org/2000/svg" width="26.003" height="28.762" viewBox="0 0 28.003 30.762">
                                            <path id="Path_2" data-name="Path 2" d="M7.552,27.348a.231.231,0,0,1-.305-.047L4.635,24.19c-.07-.087-.078-.134.016-.266a49.448,49.448,0,0,1,8.764-9.506c.047-.031.031-.071-.024-.047A115.564,115.564,0,0,0,0,20.234V30.8H28V20.625c-3.815.648-15.245,4.05-20.444,6.723ZM18.934,8.335a.258.258,0,0,1,.289.109c.485.539,2.034,2.431,2.455,2.947a.174.174,0,0,1-.038.25,55.852,55.852,0,0,0-8.545,8.365c-.038.055-.007.055.024.047A97.2,97.2,0,0,1,28,15.629V11.1L14.018.04.008,11.094v4.948A98.363,98.363,0,0,1,18.934,8.335Z" transform="translate(0 -0.04)" fill="#b2b2db" />
                                        </svg>
                                    </a>
                                    <md-tooltip>
                                        <h4 class="text-primary description my-0">Zillow</h4>
                                        <p class="lighter-description description-normal"></p>
                                    </md-tooltip>
                                </span>
                            </div>
                            <div class="d-inline-block mb-2 px-2">
                                <span v-if="socialForm.facebook">
                                    <a :href="socialForm.facebook" class="social-icon icon-facebook d-flex align-center justify-content-center">
                                        <i class="fab fa-facebook-f"></i>
                                    </a>
                                    <md-tooltip>
                                        <h4 class="text-primary description my-0">Facebook</h4>
                                        <p v-if="socialForm.facebook" class="lighter-description description-normal">{{socialForm.facebook}}</p>
                                    </md-tooltip>
                                </span>
                                <span v-else>
                                    <a class="social-icon icon-facebook d-flex align-center justify-content-center">
                                        <i class="fab fa-facebook-f"></i>
                                    </a>
                                    <md-tooltip>
                                        <h4 class="text-primary description my-0">Facebook</h4>
                                        <p class="lighter-description description-normal"></p>
                                    </md-tooltip>
                                </span>
                            </div>
                            <div class="d-inline-block icon-video mb-2 px-2">
                                <span v-if="socialForm.youtube">
                                    <a :href="socialForm.youtube" class="social-icon icon-video d-flex align-center justify-content-center">
                                        <svg xmlns="http://www.w3.org/2000/svg" width="28" height="28" viewBox="0 0 33 33">
                                            <g id="video" transform="translate(1.5 1.5)">
                                                <path id="Path" d="M13.5,7.5,0,14.85V0Z" transform="translate(10.5 7.5)" fill="none" stroke="#b2b2db" stroke-linecap="square" stroke-miterlimit="10" stroke-width="3" />
                                                <circle id="Oval" cx="15" cy="15" r="15" fill="none" stroke="#b2b2db" stroke-linecap="square" stroke-miterlimit="10" stroke-width="3" />
                                            </g>
                                        </svg>
                                    </a>
                                    <md-tooltip>
                                        <h4 class="text-primary description my-0">Youtube</h4>
                                        <p v-if="socialForm.youtube" class="lighter-description description-normal">{{socialForm.youtube}}</p>
                                    </md-tooltip>
                                </span>
                                <span v-else>
                                    <a class="social-icon icon-video d-flex align-center justify-content-center">
                                        <svg xmlns="http://www.w3.org/2000/svg" width="28" height="28" viewBox="0 0 33 33">
                                            <g id="video" transform="translate(1.5 1.5)">
                                                <path id="Path" d="M13.5,7.5,0,14.85V0Z" transform="translate(10.5 7.5)" fill="none" stroke="#b2b2db" stroke-linecap="square" stroke-miterlimit="10" stroke-width="3" />
                                                <circle id="Oval" cx="15" cy="15" r="15" fill="none" stroke="#b2b2db" stroke-linecap="square" stroke-miterlimit="10" stroke-width="3" />
                                            </g>
                                        </svg>
                                    </a>
                                    <md-tooltip>
                                        <h4 class="text-primary description my-0">Youtube</h4>
                                        <p class="lighter-description description-normal"></p>
                                    </md-tooltip>
                                </span>
                            </div>
                            <div class="d-inline-block mb-2 px-2 pr-0">

                                <span v-if="socialForm.instagram">
                                    <a :href="socialForm.instagram" class="social-icon icon-instagram d-flex align-center justify-content-center">
                                        <i class="fab fa-instagram"></i>
                                    </a>
                                    <md-tooltip>
                                        <h4 class="text-primary description my-0">Instagram</h4>
                                        <p v-if="socialForm.instagram" class="lighter-description description-normal">{{socialForm.instagram}}</p>
                                    </md-tooltip>
                                </span>

                                <span v-else>
                                    <a class="social-icon icon-instagram d-flex align-center justify-content-center">
                                        <i class="fab fa-instagram"></i>
                                    </a>
                                    <md-tooltip>
                                        <h4 class="text-primary description my-0">Instagram</h4>
                                        <p class="lighter-description description-normal"></p>
                                    </md-tooltip>
                                </span>
                            </div>
                        </div>
                    </div>
                    <div class="mb-5 d-block w-100">
                        <h4 class="text-primary mb-3">Change password</h4>
                        <div class="custom-shadow bg-white radius-10 px-4 py-4">
                            <form @submit.prevent="changePassword">
                                <div class="md-layout password-icon">
                                    <div class="md-layout-item md-medium-size-33 md-small-size-100 md-xsmall-size-100 mx-auto">
                                        <div class="text-left">
                                            <md-field slot="inputs" class="password-eye pr-4" :class="app.getValidationClass($v.changePasswordForm,'currentPassword')">
                                                <label>Current Password</label>
                                                <md-input v-model="changePasswordForm.currentPassword" type="password" @blur="$v.changePasswordForm.$touch()" />
                                                <span v-if="!$v.changePasswordForm.currentPassword.required" class="md-error">Current password is required</span>

                                                <span v-if="!$v.changePasswordForm.currentPassword.isMatched" class="md-error">Not matched</span>
                                            </md-field>
                                        </div>
                                    </div>
                                    <div class="md-layout-item md-medium-size-33 md-small-size-50 md-xsmall-size-100 mx-auto">
                                        <div class="text-left">
                                            <md-field slot="inputs" class="password-eye pr-4" :class="app.getValidationClass($v.changePasswordForm,'newPassword')">
                                                <label>New Password</label>
                                                <md-input v-model="changePasswordForm.newPassword" type="password" />
                                                <span v-if="!$v.changePasswordForm.newPassword.required" class="md-error">New password is required</span>
                                                <div v-if="!$v.changePasswordForm.newPassword.minLength">Password must be at least 6 characters</div>
                                            </md-field>
                                        </div>
                                    </div>
                                    <div class="md-layout-item md-medium-size-33 md-small-size-50 md-xsmall-size-100 mx-auto">
                                        <div class="text-left">
                                            <md-field slot="inputs" class="password-eye pr-4" :class="app.getValidationClass($v.changePasswordForm,'confirmPassword')">
                                                <label>Confirm new password</label>
                                                <md-input v-model="changePasswordForm.confirmPassword" type="password" />
                                                <span v-if="!$v.changePasswordForm.confirmPassword.required" class="md-error">Confirm password is required</span>
                                                <span v-else-if="!$v.changePasswordForm.confirmPassword.sameAsPassword">Passwords must match</span>
                                            </md-field>
                                        </div>
                                    </div>
                                </div>
                                <div class="md-layout">
                                    <div class="md-layout-item md-medium-size-33 md-small-size-50 md-xsmall-size-100">
                                        <div class="text-left">
                                            <md-button type="submit" class="md-primary w-100 button-primary-custom m-0">
                                                <LoadingButtonLoader :typeEnable="app.checkType('changePasswordLoader')" v-bind:enable="app.loading" class="mr-1" /> Update
                                            </md-button>

                                        </div>
                                    </div>
                                    <div class="md-layout-item md-medium-size-33 md-small-size-50 md-xsmall-size-100"></div>
                                    <div class="md-layout-item md-medium-size-33 md-small-size-50 md-xsmall-size-100"></div>
                                </div>
                                <!-- {{ $v.changePasswordForm }} -->
                            </form>
                        </div>
                    </div>
                </div>
                <div class="md-layout-item md-size-30 md-medium-size-100 md-small-size-100 md-hide">
                    <h4 class="text-primary mb-3">Reminder email</h4>

                    <form @submit.prevent="updateReminderEmails">

                        <div class="custom-shadow bg-white radius-10 px-4 py-3">
                            <div class="md-layout">
                                <div class="md-layout-item md-medium-size-50 md-small-size-100 md-xsmall-size-100">
                                    <md-radio v-model="radio" value="my-radio">
                                        <span class="text-primary">24 hour</span>
                                    </md-radio>
                                </div>
                                <div class="md-layout-item md-medium-size-50 md-small-size-100 md-xsmall-size-100">
                                    <md-radio v-model="radio" value="my-radio1">
                                        <span class="text-primary">48 hour</span>
                                    </md-radio>
                                </div>
                                <div class="md-layout-item md-medium-size-50 md-small-size-100 md-xsmall-size-100">
                                    <md-radio v-model="radio" value="my-radio2">
                                        <span class="text-primary">72 hour</span>
                                    </md-radio>
                                </div>
                                <div class="md-layout-item md-medium-size-50 md-small-size-100 md-xsmall-size-100">
                                    <md-radio v-model="radio" value="my-radio3">
                                        <span class="text-primary">96 hour</span>
                                    </md-radio>
                                </div>
                                <div class="md-layout-item md-medium-size-50 md-small-size-100 md-xsmall-size-100">
                                    <md-button type="submit" class="md-primary button-primary-custom mb-3 mt-3 mx-0">Update</md-button>
                                </div>
                            </div>
                        </div>
                    </form>

                </div>
            </div>

        </div>

    </div>
</div>
</template>

<script>
import {
    HTTP
} from '../../httpCommon';
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
    ); //phone valid
const isPhone = value => /^(?=.*[0-9])[- +()0-9]+$/.test(value); //phone valid
const isCharacterValid = value =>
    /^[^~!@#$%\^&*()_+={}|[\]\\:';]*$/.test(value); //special character valid
// based on https://github.com/baianat/vee-validate/blob/2.0.6/src/rules/dimensions.js
// and https://github.com/baianat/vee-validate/blob/2.0.6/locale/en.js#L18
const maxDimensionsRule = {
    getMessage(field, [width, height], data) {
        return (
            (data && data.message) ||
            `The ${field} field must be UP TO ${width} pixels by ${height} pixels.`
        );
    },
    validate(files, [width, height]) {
        const validateImage = (file, width, height) => {
            const URL = window.URL || window.webkitURL;
            return new Promise(resolve => {
                const image = new Image();
                image.onerror = () =>
                    resolve({
                        valid: false
                    });
                image.onload = () =>
                    resolve({
                        valid: image.width <= Number(width) && image.height <= Number(height) // only change from official rule
                    });

                image.src = URL.createObjectURL(file);
            });
        };
        const list = [];
        for (let i = 0; i < files.length; i++) {
            // if file is not an image, reject.
            if (!/\.(jpg|svg|jpeg|png|bmp|gif)$/i.test(files[i].name)) {
                return false;
            }
            list.push(files[i]);
        }
        return Promise.all(list.map(file => validateImage(file, width, height)));
    }
};

export default {
    name: "UserProfile",
    props: ["app"],
    data() {
        return {
            showDialog: false,
            showSocialLinks: false,
            radio: "my-radio",
            image: "",
            activeClass: 'image-exists',
            errorClass: 'image-not-exists',
            userInfo: "",
            userForm: {
                first_name: "",
                last_name: "",
                email: "",
                phone: "",
                role_id: "",
                title: "",
                licence_number: ""
            },
            changePasswordForm: {
                currentPassword: "",
                newPassword: "",
                confirmPassword: ""
            },
            socialForm: {
                website: "",
                facebook: "",
                instagram: "",
                zillow: "",
                youtube: ""
            },
            isActive: true,
            error: null,
            maxSize: 1024,
            dialogActive: false
        };
    },
    components: {
        LoadingBar,
        LoadingButtonLoader
    },
    validations: {
        userForm: {
            first_name: {
                required
            },
            last_name: {
                required
            },
            email: {
                required,
                email
            },
            phone: {
                required,
                phoneValid: isPhone,
                minLength: minLength(10),
                maxLength: maxLength(13),
            },
            title: {},
            licence_number: {}
        },
        changePasswordForm: {
            currentPassword: {
                required,
                isMatched(value) {
                    // if (value === '' || value === this.origUsername) return true
                    // this.checkingUsername = true

                    return HTTP
                        .get("api/users/checkcurrentPassword", {
                            params: {
                                password: value
                            }
                        })
                        .then(res => {
                            return res.data; //res.data has to return true or false after checking if the username exists in DB
                        });
                }
            },
            newPassword: {
                required,
                minLength: minLength(6)
            },
            confirmPassword: {
                required,
                sameAsPassword: sameAs("newPassword")
            }
        },
        socialForm: {
            website: {
                urlValid: isUrl
            },
            facebook: {
                urlValid: isUrl
            },
            instagram: {
                urlValid: isUrl
            },
            zillow: {
                urlValid: isUrl
            },
            youtube: {
                urlValid: isUrl
            }
        }
    },
    mounted() {
        // console.log(this);
        // console.log(this.$toasted);
        this.init();
    },
    computed: {
        classObject: function () {
            return {
                'image-exists': this.image,
                'image-not-exists': !this.image
            }
        }
    },
    methods: {
        init() {
            HTTP
                .get("api/users/usersInfo")
                .then(response => {
                    this.image = response.data.Image;
                    this.userInfo = response.data.User;

                    //fill user profile
                    this.userForm = {
                        first_name: this.userInfo.first_name,
                        last_name: this.userInfo.last_name,
                        email: this.userInfo.email,
                        phone: this.userInfo.phone,
                        role_id: this.userInfo.role_id,
                        licence_number: this.userInfo.licence_number,
                        title: this.userInfo.title
                    };

                    //fill user social info
                    let social = this.userInfo.social;

                    if (social) {
                        this.socialForm = JSON.parse(social);

                        this.socialForm = {
                            website: this.socialForm.website ? this.socialForm.website : '',
                            facebook: this.socialForm.facebook ? this.socialForm.facebook : '',
                            instagram: this.socialForm.instagram ? this.socialForm.instagram : '',
                            zillow: this.socialForm.zillow ? this.socialForm.zillow : '',
                            youtube: this.socialForm.youtube ? this.socialForm.youtube : ''
                        }
                    } else {
                        this.socialForm = {
                            website: "",
                            facebook: "",
                            instagram: "",
                            zillow: "",
                            youtube: ""
                        }
                    }
                })
                .catch(error => {
                    // console.log(error);
                    // if (error.response.data.errors.Message) {
                    //     if (error.response.data.errors.Message == "Token is Expired" || error.response.data.errors.Message == "Token is Invalid") {
                    //         this.$toasted
                    //             .show(error.response.data.errors.Message, {
                    //                 type: "success"
                    //                 // icon: "error_outline"
                    //             })
                    //             .goAway(1500);
                    //         localStorage.removeItem("auth");
                    //         this.$router.push("/auth/login");
                    //         return false;
                    //     }
                    // }
                });
        },
        editProfile() {
            this.showDialog = true;
        },
        updateProfile(e) {
            this.$v.userForm.$touch();
            if (this.$v.userForm.$error) return;
            this.app.loading = true;
            // this.app.loaderType = this.app.checkType("simpleLoader");
            const data = {
                first_name: this.userForm.first_name,
                last_name: this.userForm.last_name,
                email: this.userForm.email,
                phone: this.userForm.phone,
                title: this.userForm.title,
                licence_number: this.userForm.licence_number
            };
            HTTP
                .post("api/users/updateProfile", data)
                .then(response => {
                    if (response.data.Status) {
                        this.app.loading = false;
                        this.$toasted
                            .show(response.data.Message, {
                                type: "success"
                                // icon: "error_outline"
                            })
                            .goAway(1500);
                    }
                })
                .catch(error => {
                    let message = "";
                    this.app.loading = false;
                    if (error.response.data) {
                        if (error.response.data.errors) {
                            if (error.response.data.errors.email) {
                                message = error.response.data.errors.email[0];
                            } else {
                                message = error.response.data.errors;
                            }

                            this.$toasted
                                .show(message, {
                                    type: "error"
                                    // icon: "error_outline"
                                })
                                .goAway(1500);

                            if (error.response.data.errors.Message) {
                                if (
                                    error.response.data.errors.Message == "Token is Expired" ||
                                    error.response.data.errors.Message == "Token is Invalid"
                                ) {
                                    localStorage.removeItem("auth");
                                    this.$router.push("/auth/login");
                                }
                            }
                        }
                    }
                });
        },
        editSocial() {
            this.showSocialLinks = true
        },
        updateSocialLinks(e) {
            this.$v.socialForm.$touch();
            if (this.$v.socialForm.$error) return;
            this.app.loading = true;

            const data = {
                social: {
                    website: this.socialForm.website,
                    facebook: this.socialForm.facebook,
                    instagram: this.socialForm.instagram,
                    zillow: this.socialForm.zillow,
                    youtube: this.socialForm.youtube
                }
            };
            HTTP
                .post("api/users/updateSocial", data)
                .then(response => {
                    if (response.data.Status) {
                        this.app.loading = false;
                        this.$toasted
                            .show(response.data.Message, {
                                type: "success"
                                // icon: "error_outline"
                            })
                            .goAway(1500);
                    }
                })
                .catch(error => {
                    let message = "";
                    this.app.loading = false;
                    if (error.response.data) {
                        if (error.response.data.errors) {
                            if (error.response.data.errors.email) {
                                message = error.response.data.errors.email[0];
                            } else {
                                message = error.response.data.errors;
                            }

                            this.$toasted
                                .show(message, {
                                    type: "error"
                                    // icon: "error_outline"
                                })
                                .goAway(1500);

                            if (error.response.data.errors.Message) {
                                if (
                                    error.response.data.errors.Message == "Token is Expired" ||
                                    error.response.data.errors.Message == "Token is Invalid"
                                ) {
                                    localStorage.removeItem("auth");
                                    this.$router.push("/auth/login");
                                }
                            }
                        }
                    }
                });
        },
        changePassword(e) {
            this.$v.changePasswordForm.$touch();
            if (this.$v.changePasswordForm.$error) return;
            this.app.loading = true;

            const data = {
                password: this.changePasswordForm.newPassword,
            }
            HTTP
                .put("api/users/changePassword", data)
                .then(response => {
                    if (response.data.Status) {
                        this.app.loading = false;
                        this.$toasted
                            .show(response.data.Message, {
                                type: "success"
                            })
                            .goAway(1500);
                    }
                })
                .catch(error => {
                    console.log(error);
                    let message = "";
                    this.app.loading = false;

                    if (error.response.data) {
                        if (error.response.data.errors) {
                            if (error.response.data.errors.email)
                                message = error.response.data.errors.email[0];
                            if (error.response.data.errors.Message)
                                message = error.response.data.errors.Message;
                            else message = error.response.data.errors;
                        }
                    }

                    this.$toasted
                        .show(message, {
                            type: "error"
                            // icon: "error_outline"
                        })
                        .goAway(2500);
                });
        },
        updateReminderEmails(e) {

        },
        deleteProfileImg() {
            this.dialogActive = true;
        },
        onConfirm() {
            // this.value = 'Agreed'
            HTTP
                .delete("api/users/profile/image")
                .then(response => {
                    if (response.data.Status) {
                        this.app.loading = false;
                        this.image = "";

                        this.$root.$emit('profileImagEvent', {
                            profileImage: ""
                        });

                        this.$toasted
                            .show(response.data.Message, {
                                type: "success"
                            })
                            .goAway(1500);
                    }
                })
                .catch(error => {
                    console.log(error);
                    let message = "";
                    this.app.loading = false;

                    if (error.response.data) {
                        if (error.response.data.errors) {
                            if (error.response.data.errors.email)
                                message = error.response.data.errors.email[0];
                            if (error.response.data.errors.Message)
                                message = error.response.data.errors.Message;
                            else message = error.response.data.errors;
                        }
                    }

                    this.$toasted
                        .show(message, {
                            type: "error"
                            // icon: "error_outline"
                        })
                        .goAway(2500);
                });
        },
        onCancel() {
            // this.value = 'Disagreed'
            console.log('Disagreed');
        },
        onImageChange(e) {
            let files = e.target.files || e.dataTransfer.files;
            let imageFile = files[0];

            if (!files.length) return;
            const {
                maxSize
            } = this;
            let size = imageFile.size / maxSize / maxSize;
            console.log(size);
            if (!imageFile.type.match('image.*')) {
                // check whether the upload is an image
                // this.errorDialog = true
                // this.errorText = 'Please choose an image file';
                this.$toasted
                    .show('Please choose an image file', {
                        type: "success"
                        // icon: "error_outline"
                    }).goAway(2500);

                return false;

            }

            if (size > 1) {
                //check whether the size is greater than the size limit
                // this.errorDialog = true;
                // this.errorText = 'Your file is too big! Please select an image under 1MB';

                this.$toasted
                    .show('Your file is too big! Please select an image under 1MB', {
                        type: "success"
                        // icon: "error_outline"
                    }).goAway(2500);

                return false;
            }

            let formData = new FormData()

            formData.append('image', imageFile)

            HTTP.post('api/users/profileImage', formData)
                .then(resp => {
                    // console.log(resp);
                    this.image = resp.data.Image;
                    // console.log(resp.data.Image);

                    this.$root.$emit('profileImagEvent', {
                        profileImage: resp.data.Image
                    });

                    this.$toasted
                        .show(resp.data.Message, {
                            type: "success"
                            // icon: "error_outline"
                        }).goAway(1500);
                }).catch(function (error) {

                    let message = "";
                    // this.imageLoader = false;
                    if (error.response.data) {
                        if (error.response.data.errors) {
                            if (error.response.data.errors.email) {
                                message = error.response.data.errors.email[0];
                            } else {
                                message = error.response.data.errors;
                            }

                            this.$toasted
                                .show(message, {
                                    type: "error"
                                    // icon: "error_outline"
                                })
                                .goAway(1500);

                        }
                    }
                });

        },
        createImage(file) {
            let reader = new FileReader();
            let vm = this;
            reader.onload = e => {
                vm.image = e.target.result;
            };
            reader.readAsDataURL(file);

            this.uploadImage();
        },
        uploadImage() {
            // this.imageLoader = true;
            const config = {
                headers: {
                    "content-type": "multipart/form-data"
                }
            };

            let formData = new FormData();
            formData.append("image", this.image);
            let _this = this;

            HTTP
                .post("api/users/profileImage", formData)
                .then(function (response) {
                    // _this.$root.$emit('profileImagEvent', {
                    //     profileImage: response.Image
                    // });

                    _this.$toasted
                        .show(response.Message, {
                            type: "success"
                            // icon: "error_outline"
                        }).goAway(1500);
                })
                .catch(function (error) {
                    let message = "";
                    // this.imageLoader = false;
                    if (error.response.data) {
                        if (error.response.data.errors) {
                            if (error.response.data.errors.email) {
                                message = error.response.data.errors.email[0];
                            } else {
                                message = error.response.data.errors;
                            }

                            // _this.$toasted
                            //     .show(message, {
                            //         type: "error"
                            //         // icon: "error_outline"
                            //     })
                            //     .goAway(1500);

                        }
                    }
                });
        }
    }
};
</script>

<style lang="scss" scoped>
.md-dialog {
    width: 700px;
    border-radius: 10px;
}
</style>
