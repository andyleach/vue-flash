<template>
    <div class="alert-wrap">
        <transition-group :name="transition" tag="div">
            <div :class="item.classObject" role="alert" :key="index" v-show="item.show" v-for="(item, index) in notifications">
                <span class="icon" v-if="bulma">
                    <i class="fa" :class="item.iconObject" v-if="icon"></i>
                </span>

                <i class="fa" :class="item.iconObject" v-if="icon && !bulma"></i>

                <span v-html="item.message"></span>
            </div>
        </transition-group>
    </div>
</template>
<script>
    export default {
        props: {
            timeout: {
                type: Number,
                default: 3000
            },
            transition: {
                type: String,
                default: 'slide-fade'
            },
            bulma: {
                type: Boolean,
                default: false
            },
            icon: {
                type: Boolean,
                default: false
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
                this.notifications.push({
                    message: message,
                    type: type,
                    classObject: this.classes(type),
                    iconObject: this.icons(type),
                    show: true
                });
                setTimeout(this.hide, this.timeout);
            },

            /**
             * Sets and returns the values for our notification item's classObject
             *
             * @param type
             */
            classes(type) {
                let prefix = 'alert';
                let classes = {
                    'alert': true,
                    'alert-danger':  false,
                    'alert-success': false,
                    'alert-info':    false,
                    'alert-warning': false,
                };

                if(this.bulma){
                    classes = {
                        'notification': true,
                        'is-danger':  false,
                        'is-success': false,
                        'is-info':    false,
                        'is-warning': false,
                    };
                    prefix = 'is';
                }

                classes[prefix+'-'+(type === 'error' ? 'danger' : type)] = true;

                return classes;
            },
            /**
             * Sets and returns the values for our notification item's icon
             *
             * @param type
             */
            icons(type) {

                let icons = {
                    'danger':  'fa-exclamation-circle',
                    'success': 'fa-check-circle',
                    'info':    'fa-info-circle',
                    'warning': 'fa-exclamation-circle',
                };

                return icons[type];
            },

            /**
             * Hide Our Alert
             *
             * @param item
             */
            hide(item = this.notifications[0]) {
                let key = this.notifications.indexOf(item);
                this.notifications[key].show = false;
                this.notifications.splice(key, 1);
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
