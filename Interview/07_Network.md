# Network

## 🎯 Goal

- Client와 Server의 역할과 책임을 구분하고 게임 네트워크의 기본 구조를 설명할 수 있다.
- Server Authority의 의미와 중요한 게임 로직을 Server에서 처리하는 이유를 설명할 수 있다.
- TCP와 UDP의 차이를 신뢰성, 순서 보장, 지연 및 실시간성 관점에서 설명할 수 있다.
- RPC의 개념과 Client → Server / Server → Client 통신에서의 활용 방식을 설명할 수 있다.
- 게임 상태 동기화의 개념과 Client / Server 상태가 달라지는 원인을 설명할 수 있다.
- Latency, Ping, Packet Loss를 구분하고 각각이 게임에 미치는 영향을 설명할 수 있다.
- Packet의 개념과 패킷에 포함될 수 있는 게임 데이터를 설명할 수 있다.
- Serialization / Deserialization의 의미와 네트워크 데이터 변환 과정을 설명할 수 있다.
- Interpolation, Client Prediction, Reconciliation의 목적과 차이를 설명할 수 있다.
- 실시간 게임에서 네트워크 지연과 패킷 유실에 대응하는 기본적인 방법을 설명할 수 있다.
- 데이터의 중요도와 실시간성에 따라 적절한 통신 및 동기화 방식을 선택하는 관점을 갖는다.

---

# 1. Client / Server / Server Authority

## Q1. Client와 Server란 무엇인가요?

- Game Logic / DB

### 답변

Client는 플레이어가 직접 보는 화면과 입력 및 즉각적인 게임 연출 등을 담당하고,
Server는 여러 Client가 공유해야 하는 게임 로직과 데이터를 처리합니다.
중요한 데이터는 Server에서 관리하여 보안과 데이터 일관성을 확보할 수 있습니다.

---

## Q2. 게임에서 중요한 로직을 Server에서 처리하는 이유는 무엇인가요?

- Server Authority
- Synchronization
- Security
- DB

### 답변

여러 Client가 동일한 결과를 공유해야 하고, 중요한 게임 데이터를 Server에서 관리해야 하기 때문입니다. 또한 Client가 결과를 직접 결정하지 못하도록 하여 게임 데이터의 위변조를 방지할 수 있습니다.

---

## Q3. Client가 게임 결과를 직접 결정하면 어떤 문제가 발생할 수 있나요?

- Security / Trust
- Synchronization

### 답변

Client마다 다른 결과가 발생하여 게임 상태가 달라질 수 있고, Client가 데이터를 임의로 수정할 가능성도 생깁니다. 따라서 중요한 판정과 데이터는 Server가 최종적으로 결정하는 구조가 필요합니다.

---

## Q4. 플레이어가 공격 버튼을 눌렀을 때 Client와 Server 사이에서 어떤 과정으로 처리될 수 있나요?

### 답변

Client에서 공격 입력과 캐릭터의 즉각적인 연출을 처리하고, 필요한 공격 정보를 Server에 전달합니다. Server는 데미지 등 최종 게임 결과를 판정한 뒤 그 결과를 Client에 전달하고, Client는 결과에 맞춰 게임 화면을 갱신합니다.

---

## Q5. 모든 게임 로직을 Server에서 처리하는 것이 항상 좋은 방법인가요?

- Client Processing
- Server Load
- Real-time Response

### 답변

아닙니다. 즉각적인 화면 연출이나 Client에서만 필요한 처리는 Client에서 수행하는 것이 적절합니다. Server에서는 여러 사용자가 공유해야 하는 데이터와 중요한 판정을 중심으로 처리하여 Server 부담과 불필요한 통신을 줄일 수 있습니다.

---

# 2. Serialization / Packet

## Q1. 패킷(Packet)이란 무엇인가요?

- Data Unit

### 답변

패킷은 네트워크를 통해 데이터를 전달하기 위해 일정한 단위로 나눈 데이터 묶음입니다. 게임에서는 플레이어 입력, 캐릭터 상태, 전투 결과 등의 정보를 패킷에 담아 전달할 수 있습니다.

---

## Q2. 게임에서 하나의 패킷에는 어떤 정보가 포함될 수 있나요?

- Input
- Character State
- Combat Data

### 답변

공격 입력, 캐릭터 위치나 상태, 공격 대상과 데미지 결과, 아이템 사용 정보 등 게임에서 전달해야 하는 다양한 데이터를 포함할 수 있습니다.

---

## Q3. 게임 네트워크에서 Serialization과 Deserialization이란 무엇인가요?

### 답변

Serialization은 게임에서 사용하는 객체나 데이터를 네트워크로 전송할 수 있는 형태로 변환하는 과정입니다. Deserialization은 전달받은 데이터를 다시 게임에서 사용할 수 있는 객체나 데이터 형태로 변환하는 과정입니다.

---

## Q4. 패킷 크기가 너무 커지면 어떤 문제가 발생할 수 있나요?

- Bandwidth
- Network Load

### 답변

패킷 크기가 너무 커지면 한 번에 전송해야 하는 데이터량이 증가하기 때문에 네트워크 대역폭과 처리 부담이 커질 수 있습니다. 실시간 게임에서는 이러한 부담이 지연이나 통신 품질 저하로 이어질 수 있습니다.

---

## Q5. 게임 네트워크에서 데이터 전송량을 줄이려면 어떤 방법을 사용할 수 있나요?

- Event-driven
- Data Reduction
- Packet Size

### 답변

모든 데이터를 계속 전송하기보다는 실제 변경이나 이벤트가 발생했을 때 필요한 데이터만 전달하고, 패킷에 불필요한 데이터를 포함하지 않도록 구성할 수 있습니다. 또한 패킷 크기를 적절하게 유지하는 것도 중요합니다.

---

# 3. TCP / UDP

## Q1. TCP와 UDP는 무엇인가요?

- Network Protocol
- Reliability

### 답변

TCP와 UDP는 데이터를 전송하기 위한 네트워크 프로토콜입니다. TCP는 데이터의 신뢰성과 순서를 보장하는 방식이고, UDP는 이러한 보장을 기본적으로 제공하지 않는 대신 상대적으로 가볍고 빠른 통신에 적합합니다.

---

## Q2. TCP에서 데이터를 보낸 순서와 받은 순서를 보장하는 이유는 무엇인가요?

### 답변

TCP는 데이터를 전송 과정에서 여러 단위로 나누어 전달할 수 있기 때문에 수신 측에서 올바른 데이터 순서로 재구성할 수 있도록 순서를 관리합니다.

---

## Q3. 실시간 게임에서 UDP를 사용하는 이유는 무엇인가요?

- Low Overhead

### 답변

실시간 게임에서는 오래된 데이터보다 최신 데이터가 중요한 경우가 많기 때문에, 재전송이나 순서 보장에 따른 지연을 줄이고 빠르게 데이터를 전달할 수 있는 UDP가 적합할 수 있습니다.

---

## Q4. 실시간 게임에서 TCP를 사용하면 어떤 문제가 발생할 수 있나요?

- Delay

### 답변

데이터의 신뢰성과 순서를 보장하는 과정에서 추가적인 지연이 발생할 수 있습니다. 실시간성이 중요한 데이터에서는 이러한 지연이 게임 반응성에 영향을 줄 수 있습니다.

---

## Q5. UDP 사용하면서 데이터의 신뢰성이 필요한 경우에는 어떻게 처리할 수 있나요?

- Retransmission

### 답변

UDP 자체는 신뢰성을 보장하지 않기 때문에 필요한 경우 애플리케이션에서 별도의 확인이나 재전송 로직을 구현할 수 있습니다.

---

## Q6. 게임에서 TCP가 적합한 데이터와 UDP가 적합한 데이터의 예를 각각 들어주세요.

### 답변

TCP는 사용자 정보나 업적 정보처럼 정확한 전달과 순서가 중요한 데이터에 적합할 수 있습니다. UDP는 실시간성이 중요한 위치, 입력, 전투 관련 상태 등 최신 데이터가 중요한 통신에 적합할 수 있습니다.

---

## Q7. TCP와 UDP 중 어떤 것을 선택할지는 무엇을 기준으로 결정하나요?

- Reliability / Order
- Real-time

### 답변

데이터의 신뢰성과 순서 보장이 중요한지, 또는 지연을 줄이는 것이 더 중요한지를 기준으로 결정합니다.

---

# 4. RPC

## Q1. RPC란 무엇인가요?

- Remote Procedure Call
- Remote Function

### 답변

RPC(Remote Procedure Call)는 네트워크를 통해 다른 컴퓨터나 Server에 있는 함수를 원격으로 호출하는 방식입니다.

---

## Q2. Client → Server 방향 RPC와 Server → Client 방향 RPC의 예를 들어주세요?

- Client → Server
- Server → Client

### 답변

Client → Server RPC는 플레이어가 특정 행동을 요청하는 경우에 사용할 수 있습니다. 예를 들어 공격이나 아이템 사용을 Server에 요청할 수 있습니다.  
Server → Client RPC는 Server에서 발생한 이벤트를 Client에 전달하는 경우에 사용할 수 있습니다. 예를 들어 특정 이벤트나 게임 상태 변경을 여러 Client에 알릴 수 있습니다.

---

## Q3. RPC를 사용할 때 모든 데이터를 RPC로 전달하면 어떤 문제가 생길 수 있나요?

- Network Traffic

### 답변

RPC 호출이 지나치게 많아지면 네트워크 트래픽과 Server 및 Client의 처리 부담이 증가할 수 있습니다.  
따라서 RPC는 원격으로 실행해야 하는 동작에 적절히 사용하고, 지속적으로 변하는 상태 데이터는 별도의 동기화 방식이 더 적합할 수 있습니다.

---

## Q5. RPC와 데이터 동기화는 어떤 차이가 있나요?

### 답변

RPC는 특정 동작이나 함수를 원격에서 실행하도록 요청하는 개념이고, 데이터 동기화는 Client와 Server 또는 여러 Client의 게임 상태를 동일하게 맞추는 개념입니다.

---

## Q6. RPC 호출이 네트워크 문제로 정상적으로 전달되지 않는다면 어떤 상황이 발생할 수 있나요?

- Packet Loss
- Request Failure

### 답변

RPC 요청이나 이벤트가 전달되지 않아 해당 동작이 실행되지 않을 수 있습니다. 중요한 요청이라면 통신 방식에 맞는 재전송이나 확인 로직을 고려할 수 있습니다.

---

# 5. 데이터 동기화

## Q1. 게임에서 데이터 동기화란 무엇인가요?

- Game State

### 답변

Client와 Server 또는 Client와 Client 사이에서 공유해야 하는 게임 상태를 동일하게 맞추는 것을 의미합니다. 예를 들어 캐릭터 위치, 게임 상태, 결과 등의 데이터를 서로 일치시키는 과정입니다.

---

## Q2. 게임에서 모든 데이터를 실시간으로 동기화해야 하나요?

- Data Importance

### 답변

모든 데이터를 실시간으로 동기화할 필요는 없습니다. 위치나 전투 상태처럼 실시간성이 중요한 데이터는 자주 동기화할 수 있지만, Inventory나 일부 데이터처럼 즉각적인 동기화가 필요하지 않은 데이터는 상황에 맞는 주기로 처리할 수 있습니다.

---

## Q3. 플레이어의 위치를 동기화할 때 어떤 문제가 발생할 수 있나요?

- Network Delay

### 답변

네트워크 지연이나 데이터 전달 주기의 차이로 Client가 가진 위치와 Server 또는 다른 Client가 가진 위치에 차이가 발생할 수 있습니다.

---

## Q4. 네트워크 지연으로 Client와 Server의 상태가 달라지면 어떻게 처리할 수 있나요?

- Server State
- Correction

### 답변

Server가 최종적인 게임 상태를 결정하는 구조라면 Server의 상태를 기준으로 Client의 상태를 보정하여 다시 동기화할 수 있습니다.

---

# 6. Latency / Ping / Packet Loss

## Q1. 게임 네트워크에서 Latency란 무엇인가요?

- Network Delay
- Response Time

### 답변

Latency는 Client와 Server 사이에서 데이터가 전달되고 처리되는 과정에서 발생하는 시간 지연을 의미합니다.

---

## Q2. Latency가 높아지면 게임에서 어떤 문제가 발생할 수 있나요?

- Delay
- Synchronization

### 답변

Client가 Server의 상태나 처리 결과를 늦게 받기 때문에 입력 반응이 늦어지거나 Client와 Server 사이의 게임 상태에 차이가 발생할 수 있습니다.

---

## Q3. Ping이란 무엇인가요?

- Round Trip Time
- Network Test

### 답변

Ping은 특정 대상에 데이터를 보내고 응답을 받아 왕복하는 데 걸리는 시간을 측정한 값입니다. 네트워크 연결 상태와 지연 시간을 확인하는 데 사용할 수 있습니다.

---

## Q4. Packet Loss란 무엇인가요?

### 답변

Packet Loss는 네트워크를 통해 전송한 패킷이 목적지에 정상적으로 도착하지 못하고 유실되는 현상을 의미합니다.

---

## Q5. Packet Loss가 발생하면 게임에서 어떤 문제가 발생할 수 있나요?

- Synchronization
- Retransmission
- Delay

### 답변

필요한 게임 상태나 입력 정보가 전달되지 않아 Client와 Server의 상태가 달라질 수 있습니다. 신뢰성이 필요한 데이터라면 재전송 등의 처리가 필요하고, 이 과정에서 추가적인 지연이 발생할 수 있습니다.

---

## Q6. 실시간 게임에서 Latency를 줄이기 위해 어떤 방법을 사용할 수 있나요?

- Network Traffic
- Data Frequency

### 답변

필요한 데이터만 전송하고 불필요하게 자주 데이터를 주고받지 않도록 하여 네트워크 통신량을 줄일 수 있습니다. 데이터 특성에 따라 TCP와 UDP를 적절히 선택하는 것도 방법입니다.

---

## Q7. Packet Loss가 발생했을 때 게임에서는 어떻게 대응할 수 있나요?

- Retransmission
- Data Importance

### 답변

데이터의 중요도에 따라 대응 방법을 다르게 적용할 수 있습니다. 반드시 전달되어야 하는 데이터라면 재전송을 통해 유실된 데이터를 다시 전달할 수 있고, 일부 유실을 허용할 수 있는 실시간 데이터라면 최신 데이터를 다시 전달하거나 상태를 보정하는 방식으로 처리할 수 있습니다.

---

# 7. 동기화 문제 대응

## Q1. Client와 Server의 상태를 부드럽게 보여주기 위해 사용하는 방법은 무엇인가요?

- Interpolation

### 답변

Interpolation은 서로 다른 시점에 전달된 상태 데이터 사이의 값을 보간하여 Client에서 움직임을 부드럽게 보여주는 방법입니다. 네트워크 지연으로 상태가 일정한 간격으로 전달되지 않더라도 화면에서 자연스럽게 움직이는 것처럼 보여주기 위해 사용합니다.

---

## Q2. Client Prediction이란 무엇이며, 왜 사용하나요?

- Input Response
- Latency

### 답변

Client Prediction은 Server의 처리 결과를 기다리는 동안 발생하는 지연을 줄이기 위해 Client가 자신의 입력에 대한 결과를 먼저 예측해서 적용하는 방식입니다. 이후 Server의 최종 결과를 받으면 실제 상태와 비교하여 필요한 경우 Client의 상태를 보정합니다.

---

## Q3. Client Prediction의 결과와 Server의 최종 결과가 다르면 어떻게 처리할 수 있나요?

- Server State
- Correction

### 답변

Client Prediction의 결과와 Server의 최종 결과가 다르면 Server의 최종 상태를 기준으로 Client의 상태를 보정하고 다시 동기화합니다.

---

## Q4. Reconciliation이란 무엇인가요?

- Reconciliation
- Prediction Correction

### 답변

Reconciliation은 Client Prediction의 결과와 Server의 최종 상태가 다를 때 Server의 권위 있는 상태를 기준으로 Client의 상태를 보정하는 과정입니다.

