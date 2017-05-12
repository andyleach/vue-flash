<template>
    <div :class="classObject" role="alert" v-show="show">
        <strong>{{ status }}</strong><br>
        {{ body }}
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
            }
        },

        data: () => ({
            body: '',
            msgtype: this.type,
            show: false,
            classObject: {
                'alert':         true,
                'alert-danger':  false,
                'alert-success': false,
                'alert-info':    false,
                'alert-warning': false,
            }
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
        computed: {
            /**
             * Set our first character of the message type to uppercase
             */
            status() {
                if(this.msgtype == 'danger') {
                    return 'Error: ';
                }

                return ucfirst(this.type + ': ');
            },
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
                this.body = message;
                this.msgtype = type;
                this.classObject['alert-'+type] = true;
                this.show = true;

                this.hide();
            },

            /**
             * Hide our Alert after 3 seconds
             */
            hide() {
                setTimeout(() => {
                    this.show = false;
                }, 3000);
            }
        },
    }
</script>
<style scoped>
    .alert {
        position: fixed;
        right: 25px;
        bottom: 25px;
        z-index:9999;
    }
</style>