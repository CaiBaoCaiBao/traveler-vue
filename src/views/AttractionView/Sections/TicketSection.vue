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
              <div class="ticket-head">
                <div class="ticket-title">
                  <div>
                    <span v-if="ticket.ticket_type === 1" class="ticket-type">单次票</span>
                    <span v-else-if="ticket.ticket_type === 2" class="ticket-type">多次票</span>
                    <span v-else-if="ticket.ticket_type === 3" class="ticket-type">联票</span>
                    <span v-else-if="ticket.ticket_type === 4" class="ticket-type">VIP票</span>
                  </div>
                  <div>{{ ticket.name }}</div>
                  <div>
                    库存：<span>{{ ticket.sold_quantity }} / {{ ticket.total_quantity }}</span>
                  </div>
                  <div>
                    预留：<span>{{ ticket.reserved_quantity }}</span>
                  </div>
                </div>

                <div class="price">
                  <div class="old_price" :class="{ discount: ticket.newPrice }">
                    <span
                      >{{ ticket.currency }} <strong>{{ ticket.oldPrice }}</strong
                      >/人</span
                    >
                  </div>
                  <div class="new_price" v-if="ticket.newPrice">
                    <span
                      >{{ ticket.currency }} <strong>{{ ticket.newPrice }}</strong
                      >/人</span
                    >
                  </div>
                </div>
              </div>
            </template>
            <div class="ticket-description" v-if="ticket.description">
              <p>{{ ticket.description }}</p>
            </div>
            <div class="ticket-gird">
              <div>
                有效期：
                <span v-if="ticket.validity_type === 1"
                  >购买后{{ ticket.validityDays }}天内有效</span
                >
                <span v-else="ticket.validity_type === 2"
                  >{{ ticket.startDate }}至{{ ticket.endDate }}</span
                >
              </div>
              <div>
                使用时段：<span>{{ ticket.timeSlot }}</span>
              </div>
              <div>
                最小使用人数：<span>{{ ticket.minVisitors }}</span>
              </div>
              <div>
                最大使用人数：<span>{{ ticket.maxVisitors }}</span>
              </div>
            </div>
            <div class="ticket-gird">
              <div>
                使用人群：
                <span v-if="ticket.audience_type === 0">儿童</span>
                <span v-else-if="ticket.audience_type === 1">学生</span>
                <span v-else-if="ticket.audience_type === 2">老人</span>
                <span v-else-if="ticket.audience_type === 3">军人（含退役）</span>
                <span v-else-if="ticket.audience_type === 4">残疾人</span>
                <span v-else-if="ticket.audience_type === 5">成人</span>
                <span v-else-if="ticket.audience_type === 6">家庭</span>
              </div>
              <div>
                适用年龄：<span>{{ ticket.min_age }} - {{ ticket.max_age }}</span>
              </div>
              <div>
                需要提供的证件：<span>{{ ticket.proof_required }}</span>
              </div>
              <div>
                折扣率：<span>{{ ticket.discont_rate }}</span>
              </div>
            </div>

            <div v-if="ticket.exchangeable">
              改期规则：
              <div>{{ ticket.exchangeRules }}</div>
            </div>
            <div v-if="ticket.refundable">
              退款规则：
              <div>{{ ticket.refundRules }}</div>
            </div>
            <div v-if="ticket.notes">
              备注：
              <p>{{ ticket.notes }}</p>
            </div>
            <div class="self-end">
              <button class="edit-btn">Editor</button>
            </div>
          </el-collapse-item>
        </el-collapse>
      </div>
      <div class="self-center">
        <button class="add-btn">Add</button>
      </div>
    </div>
  </el-card>
</template>

<script setup lang="ts">
import BinaryToggle from '@/components/ToggleButton/BinaryToggle.vue'
import { reactive, ref } from 'vue'

const isFree = ref(false)
interface TicketData {
  id: string // 编号
  name: string // 名称
  ticket_type: number // 票种
  oldPrice: number // 原价
  newPrice?: number // 新价
  currency: string // 货币单位
  validity_type: number // 有效期类型
  validityDays?: number // 有效期天数
  startDate?: string // 开始日期
  endDate?: string // 结束日期
  timeSlot: string // 时间段
  minVisitors: number // 最小游客数
  maxVisitors: number // 最大游客数
  refundable: boolean // 是否可退
  refundRules?: string // 退款规则
  exchangeable: boolean // 是否可改期
  exchangeRules?: string // 改期规则
  description?: string // 描述
  notes?: string // 备注
  status: number // 状态
  audience_type: number // 受众类型
  min_age: number // 最小年龄
  max_age: number // 最大年龄
  proof_required: string // 证件要求
  discont_rate: number // 折扣率
  total_quantity: number // 总数量
  sold_quantity: number // 已售数量
  reserved_quantity: number // 预留数量
  is_available: boolean // 是否可售卖
}
const ticketData = reactive<TicketData[]>([
  {
    id: 'Ticket-1',
    name: 'Adult Ticket',
    ticket_type: 1,
    oldPrice: 100,
    newPrice: 0,
    currency: '￥',
    validity_type: 1,
    validityDays: 30,
    timeSlot: '10:00 AM - 5:00 PM',
    minVisitors: 1,
    maxVisitors: 5,
    refundable: true,
    refundRules: 'Refund within 24 hours of purchase',
    exchangeable: false,
    description: 'This ticket is valid for 30 days from the date of purchase',
    notes: 'Please bring a valid ID',
    status: 1,
    audience_type: 5, // 受众类型
    min_age: 15, // 最小年龄
    max_age: 60, // 最大年龄
    proof_required: 'ID Card', // 证件要求
    discont_rate: 0.8, // 折扣率
    total_quantity: 100, // 总数量
    sold_quantity: 20, // 已售数量
    reserved_quantity: 10, // 预留数量
    is_available: true, // 是否可售卖
  },
  {
    id: 'Ticket-2',
    name: 'Child Ticket',
    ticket_type: 2,
    oldPrice: 50,
    newPrice: 40,
    currency: '￥',
    validity_type: 2,
    startDate: '2025-6-14',
    endDate: '2025-7-30',
    timeSlot: '10:00 AM - 5:00 PM',
    minVisitors: 1,
    maxVisitors: 5,
    refundable: true,
    refundRules: 'Refund within 24 hours of purchase',
    exchangeable: true,
    exchangeRules: 'Exchange within 48 hours of purchase',
    description: 'This ticket is valid for 30 days from the date of purchase',
    notes: 'Please bring a valid ID',
    status: 1,
    audience_type: 0, // 受众类型
    min_age: 0, // 最小年龄
    max_age: 14, // 最大年龄
    proof_required: 'ID Card', // 证件要求
    discont_rate: 0.5, // 折扣率
    total_quantity: 200, // 总数量
    sold_quantity: 50, // 已售数量
    reserved_quantity: 20, // 预留数量
    is_available: true, // 是否可售卖
  },
])
const data = ref<Array<Record<string, any>>>([...ticketData])

const activeName = ref(data.value[0].id)
</script>

<style scoped lang="scss">
@mixin general() {
  display: flex;
  align-items: center;
}

.card-header {
  @include general;
  justify-content: space-between;
}

.demo-collapse {
  margin-bottom: 5px;
}

.ticket-head {
  @include general;
  width: 90%;
  user-select: none;
  justify-content: space-between;
  font-size: 1.1em;
}

.ticket-title {
  @include general;
  gap: 10px;
}

.ticket-type {
  border: 1px solid #ccc;
  border-radius: 5px;
  padding: 4px 8px;
}

.price {
  @include general;
}

.old_price.discount {
  color: #a2a2a2;
  text-decoration: line-through;
}

.ticket-gird {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr 1fr;
}

.self-end {
  justify-self: end;
}

.edit-btn {
  margin-left: 10px;
  color: #409eff;
  cursor: pointer;
}

.self-center {
  justify-self: center;
  width: 100%;
}

.add-btn {
  width: 100%;
  cursor: pointer;
}
</style>
