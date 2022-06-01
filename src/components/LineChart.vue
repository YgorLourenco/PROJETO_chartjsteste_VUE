<script>
// Importar grafico de linha para o componente
import {Line} from "vue-chartjs"

export default {
    // Configurando o grafico de linha
    extends: Line,
    props:{
        label:{
            type: String
        },
        chartData:{
            type: Array
        },
        options: {
            type: Object
        }
    },
    // Montando os dados no grafico
    mounted(){
        const dates = this.chartData.map(d => d.date).reverse()
        const totals = this.chartData.map(d => d.total).reverse()

        this.renderChart({
            labels:dates,
            datasets: ({
                label: this.label,
                data: totals
            })
        }, this.options)
    }
}
</script>