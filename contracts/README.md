# 💸 StreamPay

> **Continuous salary and subscription payments that flow per-second. Withdraw anytime.**

## 🎯 What is StreamPay?

StreamPay enables real-time money streaming on the Stacks blockchain. Instead of traditional monthly payments, funds flow continuously per-second from sender to recipient. Recipients can withdraw their earned balance at any moment without waiting for payment cycles.

Perfect for:
- 💼 **Payroll** - Stream salaries to employees in real-time
- 📺 **Subscriptions** - Charge subscribers per-second of usage
- 🤝 **Freelance** - Pay contractors as work progresses
- 💰 **Vesting** - Token vesting with per-second unlocking

## ✨ Features

- ⏱️ **Per-second streaming** - Money flows continuously based on block height
- 💵 **Withdraw anytime** - Recipients access earned funds on-demand
- ❌ **Cancelable streams** - Senders can cancel and reclaim unstreamed funds
- 📊 **Real-time tracking** - Check stream status and available balance
- 🔒 **Secure** - Funds locked in contract until streamed

## 🚀 Quick Start

### Creating a Stream

```clarity
(contract-call? .stream-pay create-stream 
  'ST1RECIPIENT 
  u100        ;; 100 micro-STX per second
  u86400      ;; for 86400 blocks (~60 days)
)
```

### Withdrawing Funds

```clarity
(contract-call? .stream-pay withdraw u0)  ;; withdraw from stream #0
```

### Canceling a Stream

```clarity
(contract-call? .stream-pay cancel-stream u0)  ;; cancel stream #0
```

## 📖 Core Functions

### Public Functions

**`create-stream`** `(recipient principal) (amount-per-second uint) (duration uint)`
- Creates a new payment stream
- Locks total funds in contract
- Returns stream ID
- Only sender can create

**`withdraw`** `(stream-id uint)`
- Withdraws available streamed balance
- Only recipient can withdraw
- Can be called multiple times

**`cancel-stream`** `(stream-id uint)`
- Cancels active stream
- Refunds unstreamed amount to sender
- Sends streamed amount to recipient
- Only sender can cancel

### Read-Only Functions

**`get-stream`** `(stream-id uint)`
- Returns complete stream details

**`get-available-balance`** `(stream-id uint)`
- Returns current withdrawable amount

**`get-user-streams`** `(user principal)`
- Returns list of stream IDs for a user

**`get-stream-status`** `(stream-id uint)`
- Returns detailed status including completion percentage

## 💡 How It Works

1. **Sender creates stream** with recipient address, rate (μSTX/second), and duration
2. **Total amount locked** in contract: `rate × duration`
3. **Funds stream continuously** based on elapsed block height
4. **Recipient withdraws** earned balance at any time
5. **Sender can cancel** early and recover unstreamed funds

### Time-Based Calculation

StreamPay uses block height as a time proxy:
- Each block ≈ 10 minutes (Bitcoin block time)
- 6 blocks ≈ 1 hour
- 144 blocks ≈ 1 day
- Amount streamed = `amount-per-second × elapsed-blocks`

## 🔧 Example Use Cases

### Monthly Salary Stream
```clarity
;; $3000/month = ~1 μSTX per second for 30 days
(create-stream 'ST1EMPLOYEE u1 u4320)  ;; 4320 blocks ≈ 30 days
```

### Subscription Service
```clarity
;; $10/month service = 0.0003 μSTX per second
(create-stream 'ST1SERVICE-PROVIDER u1 u4320)
```

### Vesting Schedule
```clarity
;; 100,000 tokens over 1 year
(create-stream 'ST1FOUNDER u23 u52560)  ;; 52560 blocks ≈ 365 days
```

## 🧪 Testing

Deploy with Clarinet:
```bash
clarinet check
clarinet test
clarinet deploy
```

## 📊 Stream Lifecycle

```
CREATE → ACTIVE → STREAMING → [WITHDRAW] → COMPLETE
                      ↓
                  [CANCEL]
```

## ⚠️ Important Notes

- Block height is used as time measurement (not exact wall-clock time)
- Minimum stream duration: 1 block
- Recipients can withdraw multiple times during stream
- Cancelled streams become inactive permanently
- Each user can have up to 100 streams

## 🤝 Contributing

This is an MVP implementation. Contributions welcome for:
- Gas optimization
- Extended stream management features
- Integration examples
- UI components

## 📄 License

MIT

---

**Built with ❤️ for the Stacks ecosystem**