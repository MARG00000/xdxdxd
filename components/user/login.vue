<template>
    <v-card class="cardLogin">
        <v-card-title class="title">Bienvenudo!! Login</v-card-title>
        <v-card-text>
            <v-row justify="center" align="center">
                <v-col cols="4" align-self="center">
                    <img class="imgLogin" src="../../assets/images/user.jpg" alt="" srcset="">
                </v-col>
                <v-col cols="8" align-self="center">
                    <v-form ref="formLogin">
                        <v-text-field label="Correo Electrónico"
                                placeholder="Correo Electrónico"
                                v-model="correoElec"
                                :rules="validarCorreo"
                                />
                        <v-text-field label="Contraseña"
                                placeholder="Contraseña"
                                v-model="password"
                                :rules="validarPassword"
                                />
                    </v-form>
                </v-col>
            </v-row>
        </v-card-text>
        <v-card-actions>
            <v-btn class="btnLogin" rounded block @click="loginBackend">
                Iniciar Sesión
            </v-btn>
        </v-card-actions>
    </v-card>
</template>

<script>
export default {
    data() {
        return {
            correoElec: '',
            validarCorreo: [
                v => !v || /^\w+([.-]?\w+)*@\w+([.-]?\w+)*(\.\w{2,3})+$/.test(v) || 'Introduce un correo electrónico válido.'
            ],
            password: '',
            validarPassword: [
                value => value.length >= 6 || 'Mínimo 6 carácteres.'
            ]
        }
    },
    methods: {
        async loginBackend () {
            const valid = this.$refs.formLogin.validate()
            if (valid) {
                const sendData = {
                    email: this.correoElectronico,
                    password: this.password
                }
                await this.$auth.loginWith('local', {
                    data: sendData
                }).then(async (res) => {
                    console.log('respuesta del back:', res)
                    if (res.data.alert === 'Seccess') {
                        this.$router.push('/dashboard')
                    } else {
                        alert('Contraseña incorrecta!!')
                    }
                }).catch((error) => {
                    console.log('error: ', error)
                })
            }else {
                alert('Introduce los datos correctamente.')
            }
        }
    }
}
</script>

<style scoped>
.cardLogin {
    background-color: #7be396;
    border-radius: 10px;
    width: 500px;
    height: 500px;
}

.imgLogin {
    width: 100%;
    height: 100%;
}

.btnLogin {
    background-color: beige  !important;
    color: black !important;
}

.title {
    font-size: 30px;
    justify-content: center;
    color: black;
    font-weight: 300px;
}
</style>