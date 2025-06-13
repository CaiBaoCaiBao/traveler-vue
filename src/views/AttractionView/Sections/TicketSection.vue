<template>
    <el-card id="ticket">
        <template #header>
            <div class="card-header">
                <div class="title">
                    <h1>Ticket</h1>
                </div>
                <div class="tooggle-btn">
                    <BinaryToggle v-model="isFree" false-label="Paid Ticket" true-label="Free Ticket" />
                </div>
            </div>
        </template>
        <div v-if="!isFree" class="ticket-info">
            <div class="demo-collapse">
                <el-collapse v-model="activeName" accordion>
                    <el-collapse-item :name="ticket.id" v-for="ticket in data" :key="ticket.id">
                        <template #title>
                            <div class="ticket-title">
                                <div>
                                    <span v-if="ticket.ticket_type === 1" class="ticket-type">单次票</span>
                                    <span v-else-if="ticket.ticket_type === 2" class="ticket-type">多次票</span>
                                    <span v-else-if="ticket.ticket_type === 3" class="ticket-type">联票</span>
                                    <span v-else-if="ticket.ticket_type === 4" class="ticket-type">VIP票</span>
                                </div>
                                <div>{{ ticket.name }}</div>
                            </div>
                        </template>
                        <div>
                            Consistent with real life: in line with the process and logic of real
                            life, and comply with languages and habits that the users are used to;
                        </div>
                        <div>
                            Consistent within interface: all elements should be consistent, such
                            as: design style, icons and texts, position of elements, etc.
                        </div>
                    </el-collapse-item>
                </el-collapse>
            </div>
        </div>
    </el-card>
</template>

<script setup lang="ts">
import BinaryToggle from "@/components/ToggleButton/BinaryToggle.vue";
import { reactive, ref } from 'vue';

const isFree = ref(false);
interface TicketData {
    id: string,
    name: string,
    ticket_type: number,
    price: number,
    currency: string,
    validity_type: number,
    validityDays?: number,
    startDate?: string,
    endDate?: string,
    timeSlot: string,
    minVisitors: number,
    maxVisitors: number,
    refundable: boolean,
    refundRules: string,
    exchangeable: boolean,
    description: string,
    notes: string,
    status: number,
}
const ticketData = reactive<TicketData[]>(
    [{
        id: "Ticket-1",
        name: "Adult Ticket",
        ticket_type: 1,
        price: 100,
        currency: "USD",
        validity_type: 1,
        validityDays: 30,
        timeSlot: "10:00 AM - 5:00 PM",
        minVisitors: 1,
        maxVisitors: 5,
        refundable: true,
        refundRules: "Refund within 24 hours of purchase",
        exchangeable: false,
        description: "This ticket is valid for 30 days from the date of purchase",
        notes: "Please bring a valid ID",
        status: 1,
    }, {
        id: "Ticket-2",
        name: "Child Ticket",
        ticket_type: 2,
        price: 50,
        currency: "USD",
        validity_type: 1,
        validityDays: 30,
        timeSlot: "10:00 AM - 5:00 PM",
        minVisitors: 1,
        maxVisitors: 5,
        refundable: true,
        refundRules: "Refund within 24 hours of purchase",
        exchangeable: false,
        description: "This ticket is valid for 30 days from the date of purchase",
        notes: "Please bring a valid ID",
        status: 1,
    }]
)
const data = ref<Array<Record<string, any>>>([...ticketData]);

const activeName = ref(data.value[0].id);
</script>

<style scoped lang="scss">
.card-header {
    display: flex;
    align-items: center;
    justify-content: space-between;
}

.ticket-title {
    display: flex;
    align-items: center;
}

.ticket-type{
    border:1px solid #ccc;
}
</style>