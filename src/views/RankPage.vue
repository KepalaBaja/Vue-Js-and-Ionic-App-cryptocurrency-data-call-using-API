<template>
    <ion-page>
        <ion-header :translucent="true">
            <ion-toolbar>
                <ion-title>Rank Page</ion-title>
            </ion-toolbar>
        </ion-header>

        <ion-content :fullscreen="true">
            <ion-button expand="block" class="ion-margin" @click="getData">
                Refresh Rankings
            </ion-button>

            <ion-list>
                <ion-item>
                    <ion-label>
                        <ion-grid>
                            <ion-row>
                                <ion-col size="3">Rank</ion-col>
                                <ion-col size="4">Name</ion-col>
                                <ion-col size="5">Harga USD</ion-col>
                            </ion-row>
                        </ion-grid>
                    </ion-label>
                </ion-item>

                <ion-item v-for="(crypto, index) in sortedCryptoData" :key="crypto.id">
                    <ion-label>
                        <ion-grid>
                            <ion-row>
                                <ion-col size="3">#{{ index + 1 }}</ion-col>
                                <ion-col size="4">{{ crypto.name }}</ion-col>
                                <ion-col size="5">${{ crypto.price_usd }}</ion-col>
                            </ion-row>
                        </ion-grid>
                    </ion-label>
                </ion-item>
            </ion-list>
        </ion-content>
    </ion-page>
</template>

<script>
import {
    IonContent,
    IonHeader,
    IonPage,
    IonTitle,
    IonToolbar,
    IonButton,
    IonList,
    IonItem,
    IonLabel,
    IonGrid,
    IonRow,
    IonCol,
} from "@ionic/vue";
import axios from "axios";

export default {
    name: "RankPage",
    components: {
        IonContent,
        IonHeader,
        IonPage,
        IonTitle,
        IonToolbar,
        IonButton,
        IonList,
        IonItem,
        IonLabel,
        IonGrid,
        IonRow,
        IonCol,
    },
    data() {
        return {
            cryptoData: [],
        };
    },
    computed: {
        sortedCryptoData() {
            return [...this.cryptoData].sort((a, b) => b.price_usd - a.price_usd);
        },
    },
    methods: {
        async getData() {
            try {
                const response = await axios.get(
                    "https://api.coinlore.net/api/tickers/"
                );

                this.cryptoData = response.data.data;
            } catch (error) {
                console.error("Error fetching data: ", error);
            }
        },
    },
    mounted() {
        this.getData();
    },
};
</script>
