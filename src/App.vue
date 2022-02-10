<template>
    <div>
        <div v-if="tankpas">
            <strong>Username: {{ tankpas.username }}</strong>
            <br>
            <strong>Tankpasnummer: {{ tankpas.tankpas_id }}</strong>
            <br>
            <strong>Spaarpunten: {{ tankpas.spaarpunten }}</strong>
        </div>
        <div v-if="error">
            {{ error }}
        </div>
    </div>
</template>

<script>
const RELATION = 'relations';
const FLEXWORKER = 'flexworkers';

export default {

    async mounted() {
        const client = window.PluginClient;

        const route = client.route().getName();
        const role = client.user().role().getName();

        try {

            // we are on the plugins show page, fetch own
            if(route === 'account.plugins.show' || route === 'backoffice.dashboard.index') {
                await this.fetchOwnTankpas();
            }

            // get current user's tankpas
            if(role === 'flexworker' && route === 'account.flexwerker.show') {
                await this.fetchOwnTankpas();
            }

            // you are efcustomer and looking at flexworkers card
            if(role === 'efcustomer' && route === 'account.flexwerker.show') {
                await this.fetchTankpas({
                    usertype: FLEXWORKER,
                    id: client.page().flexworker().getId()
                });
            }

            // you are efcustomer and looking at relation card
            if(role === 'efcustomer' && route === 'account.relatie.show') {
                await this.fetchTankpas({
                    usertype: RELATION,
                    id: client.page().relation().getId()
                });
            }

        } catch(ex) {
            console.log('error occured', ex);
            if(ex.status === 404) {
                this.error = 'Er is geen tankpas bekend.';
                return ;
            }
            this.error = 'Er is een fout opgetreden'
        }

    },
    data() {
        return {
            tankpas: null,
            error: null
        }
    },
    methods: {
        async fetchOwnTankpas(){
            const response = await fetch('/account/simplestorage/tankpassen/items/tankpas',{ headers: { 'Accept': 'application/json' } });
            if(response.status === 404) {
                throw response;
            }

            this.tankpas = await response.json();
        },
        async fetchTankpas({ usertype, id }){
            const response = await fetch(`/backoffice/${usertype}/${id}/simplestorage/tankpassen/items/tankpas`, { headers: { 'Accept': 'application/json' } });
            if(response.status === 404) {
                throw response;
            }

            this.tankpas = await response.json();
        }
    }
}
</script>
