<template>
    <div class="alert-wrap">
        <transition-group :name="transition" tag="div">
            <div :class="item.classObject" role="alert" :key="index" v-show="item.show" v-for="(item, index) in notifications">
                <strong>{{ item.type }}</strong><br>
                {{ item.message }}
            </div>
        </transition-group>
    </div>
</template>
<script>
    var ucfirst = require('ucfirst');

    export default {
        props: {
            type: {
                type: String,
                default: 'success'
            },
            message: {
                type: String,
                default: ''
            },
            timeout: {
                type: Number,
                default: 3000
            },
            transition: {
                type: String,
                default: 'slide-fade'
            }
        },

        data: () => ({
            notifications: []
        }),

        /**
         * On creation Flash a message if a message exists otherwise listen for
         * flash event from global event bus
         */
        created() {
            if(this.message && this.type) {
                this.flash(this.message, this.type);
            }

            window.events.$on(
                'flash', (message, type) => this.flash(message, type)
            );
        },

        methods: {
            /**
             * Flash our alert to the screen for the user to see
             * and begin the process to hide it
             *
             * @param message
             * @param type
             */
            flash(message, type) {
                // Update our data properties
                let item = {
                    message: message,
                    type: ucfirst(type),
                    classObject: this.classes(type),
                    show: true
                }

                this.notifications.push(item);
                this.hide(item);
            },

            /**
             * Sets and returns the values for our notification item's classObject
             *
             * @param message
             * @param type
             */
            classes(type) {
                let classes = {
                    'alert':         true,
                    'alert-danger':  false,
                    'alert-success': false,
                    'alert-info':    false,
                    'alert-warning': false,
                }

                classes['alert-'+(type == 'error' ? 'danger' : type)] = true;

                return classes;
            },

            /**
             * Hide our Alert after 3 seconds
             */
            hide(item) {
                let key = this.notifications.indexOf(item);
                setTimeout(() => {
                    this.notifications[key].show = false;
                }, this.timeout);
            }
        },
    }
</script>
<style scoped>
    .alert-wrap {
        position: fixed;
        right: 25px;
        bottom: 25px;
        z-index:9999;
    }

    /**
     * Fade transition styles
     */
    .fade-enter-active, .fade-leave-active {
        transition: opacity .5s
    }
    .fade-enter, .fade-leave-to /* .fade-leave-active in <2.1.8 */ {
        opacity: 0
    }

    /**
     * Bounce transition styles
     */
    .bounce-enter-active {
        animation: bounce-in .5s;
    }
    .bounce-leave-active {
        animation: bounce-in .5s reverse;
    }
    @keyframes bounce-in {
        0% {
            transform: scale(0);
        }
        50% {
            transform: scale(1.5);
        }
        100% {
            transform: scale(1);
        }
    }

    /**
     * Slide transition styles
     */
    .slide-fade-enter-active {
        transition: all .3s ease;
    }
    .slide-fade-leave-active {
        transition: all .3s cubic-bezier(1.0, 0.5, 0.8, 1.0);
    }
    .slide-fade-enter, .slide-fade-leave-to
        /* .slide-fade-leave-active for <2.1.8 */ {
        transform: translateX(10px);
        opacity: 0;
    }
</style>
