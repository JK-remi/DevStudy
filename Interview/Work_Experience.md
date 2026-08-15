Chapter 15. 경력 기술서
목표
Animation Sequence Editor
Hex Map Editor
AI 개발 경험
Animation & 최적화
UI / UX
서버 연동
Auto Play AI
공통 검증 질문 (왜? 구조/기술? 구현?...)

전투 시스템
Q1. Hit Reaction 시스템을 어떻게 구현했나요? 
Sequence Event로 AttackData가 포함된 Collider 소환(Projectile 포함). Hit됐을 때 AttackData에 설정된 direction에 따라 hit_r or hit_l 플레이
수정안
Animation Sequence Event를 통해 AttackData가 포함된 Collider를소환하고, Projectile도 같은 방식으로 처리했습니다. 이후 Collider에 피격되면 AttackData에 설정된 공격 방향 정보를 확인해서 Hit_R, Hit_L 같은 방향별 피격애니메이션을 재생하도록 구현했습니다.   

Q2. Grab 시스템에서는 공격자와 피격자의 위치를 어떻게 맞췄나요? 
Grab 클래스에 Grab Bone 지정(공격 / 피격). 공격자의 Bone 위치와 피격자의 Bone 위치를 통해 보간 (Late Update)
수정안
Grab 클래스에 공격자와 피격자의 Grab Bone을 각각 지정하고, 두 Bone의 위치를 기준으로 캐릭터의 위치를 보간했습니다. 이 보정은 LateUpdate에서 처리해서 애니메이션에따른 Bone 위치를 반영하면서 공격자와 피격자의 위치가 자연스럽게 맞도록 구현했습니다. 

Q3. Airborne 시스템은 어떤 방식으로 구현했나요? 
피격자 knockdown animation. (누워 있는 거 / 일어 나는 거 분리 중 눔는 거) 플레이. rigidbody 사용 보다는 내부 Fixed Update를 통해 힘을 주고, 중력이 있다는 가정하에 높이 계산 처리. 그 상태에서 한 번 더 hit된 경우 Airbone 다시 동작(단, 위로 쳐 올리는 힘은 줄어듦). 바닥에 떨어진 경우 일정 시간 지난 후 일어나는 animation play하고, 정상 동작.(knockdown 그대로 쓴 이유는 리소스 최적화) 
수정안
피격자가 공중에 뜨는 상황에서는 Knockdown 애니메이션을 활용해 누워 있는 상태와 일어나느 상태를 처리했습니다.
물리적인 Rigidbody를 직접 사용하는대신, 내부 FixedUpdate에서 힘을 적용하는 방식으로 이동을 계산했습니다. 중력이 적용된다고 가정하고 현재 높이를 계산해서 피격자의 상승과 하강을 처리했습니다. 
공중에서 다시 피격되는 경우에는 Airborne을 다시 적용하되, 반복해서 너무 높이 튀는 것을 방지하기 위해 두 번째 피격부터는 위로 올리는 힘을 줄였습니다.
바닥에 떨어진 이후에는 일정 시간이지나면 일어나는 애니메이션을 재생하고 정상 상태로 복귀하도록 했습니다.
별도의 Airborne 전용 애니메이션 리소스를 추가하지 않고, 기존 Knckdown 애니메이션을 활용한 것은 리소스 추가를 줄이기 위한 판단이었습니다.   

Q4. Combo / Random Attack 시스템에서는 여러 공격 패턴을 어떻게 처리했나요?
일반 공격 시 몬스터가 획일적으로 공격하는 거 같다는 기획 의견에 의해 작업. Attack에 여러 AttackData를 넣고 next attack이 있는 경우 combo로 판단해서 다음 state도 attack이라면 next attack 실행. 그 외에는 attack list에 있는 것 중 random으로 골라서 attack. attack data가 달라지기 보다는 data는 그대로이고, 그에 해당하는 애니메이션이 여러가지인 상황. 
수정안
일반 공격을 반복하다 보니 몬스터의 공격 패턴이 획일적으로 보인다는 기획의견이 있어서 개선했습니다.
하나의 Attack에 여러 AttackData를 등록하고, next attack이 지정되어 있고, 다음 state도 attack이라면 combo공격으로 판단해 지정된 다음 공격을 이어서 실행하도록 했습니다.
combo가 아닌 경우에는 attack list에 등록된 공격 중 하나를 random으로 선택해 실행하도록 했습니다.
이때 공격 데이터 자체가 매번 달라지는 구조라기보다는 동일한 공격 데이터를사용하면서여러 애니메이션을 선택할 수 있도록 구성해 시각적인 공격 패턴을 다양화 했습니다.    

Q5. 전투 시스템에서 AttackData를 별도의 데이터로 관리한 이유는 무엇인가요? 
내 작업물은 아니지만 attack data는 monobehaviour로 있을 필요가 없음. 단순 data만 제공. 공격자 설정, 피격자 읽음으로 처리 가능
수정안
AttackData는 공격에 필요한데이터만 제공하는 역할이기 때문에 MonoBehaviour일 필요가 없다고 생각합니다.
공격자 쪽에서 해당  데이터를 설정하고, 피격자 쪽에서는 전달받은 AttackData를 읽어서 필요한 피격 처리를 하는 구조로 이해하고 있습니다.
제가 직접 설계한 부분은 아니지만 이런 식으로 공격 데이터와실제 동작을 분리하면 데이터 자체를 재사용하기 쉽다는 장점이 있다고 볼 수 있습니다.  

Game AI
Q1. Boss AI는 어떤 구조로 구현했나요? 
AIBase 기반이지만, Update쪽 FSM 구조는 아예 갈아치움. Phase를 HP 기반으로 나눔. 1Phase는 기존 AIBase Update대로 진행. Phase 2는 스토리 진행 및 특수 UI 및 시점 바꿈. 포탑 타는 설정이여서 FPS처럼 일인칭 슈팅으로 진행. Phase3에서 다시 AIBase 기반 FSM진행 but, skill 조건에 HP n%이하일 때 사용 가능이 있어서 사용 스킬 추가.
수정안
Boss AI는 기존 AIBase를 기반으로 하되, 기존의 Update와 FSM 구조는 Boss의 Phase 구성에 맞게 수정했습니다. Phase는 Boss의 HP를 기준으로 나눴습니다.
1Phase에서는 기존 AIBase의 FSM 로직을 사용하고, 2Phase에서는 스토리 진행과 특수 UI를 보여주면서 카메라 시점을 변경했습니다. 당시 플레이어가 포탑을 타는 연출이 있어서 FPS처럼 1인칭 슈팅 방식으로 전투가 진행되도록 별도의 처리를 했습니다.
이후 3Phase에서는 다시 기존 AIBase 기반 FSM으로 돌아가도록 구성했으며, 특정 스킬은 HP가 일정 비율 이하일 때 사용할 수 있도록 조건을 추가하고 해당 스킬을 사용할 수 있게 했습니다.

Q2. 기존 AIBase의 FSM 구조를 Boss의 Phase 시스템에 적용하면서 가장 크게 변경한 부분은 무엇인가요? 
Phase마다 스킬이 달라지므로, 스킬 사용 조건 추가. Phase 2가 제일 문제였는데, UI작업 및 기존에 없던 FPS모드 전환. GameState 전환으로 했던 거 같음. BossAI도 따로 타겟 거리 측정 없이 필중으로 스킬 사용 가능하도록  IsRange 부분을 항상 참으로 했던 듯. 
수정안
Phase마다 사용할 수 있는 스킬이 달라지기 때문에 스킬 사용조건에 Phase 조건을 추가했습니다.
특히 Phase2가 기존 FSM구조와 가장 다른 부분이었습니다. UI연출과 함께 기존 전투 방식에서 플레이어가 포탑을 조작하는 FPS 형태의 전투로 전환해야 했기 때문에 GameState를 전환해서 기존 BossAI의 일반적인 전투 흐름과 분리했습니다.
또한 Phase2에서는 Boss의스킬이 거리와 관계없이 플레이어에게 적용되는 연출이었기 때문에 일반적인 거리 기반 스킬 판정과 다르게 처리했습니다.   

Q3. Auto Play AI는 어떤 방식으로 플레이어의 다음 행동을 결정하고 이동하도록 구현했나요? 
일반적으로는 던전 진입. 구역 하나씩 진행하면서 goal까지 이동이였으므로, level에 spawner 차례대로 배치하고, spawner (collider trigger 처리) 밟은 경우 전투 끝나면 다음 spawner 가리키도록 처리. random map도 있었는데, 이건 내 작업은 아니지만 입출구 위치 알고 있으므로, 또한 random map은 하나하나의 block이므로, 길찾기를 통한 다음 block 가리키는 걸로 알고 있음.
quest 도입 후에는 던전 - 일반 맵이 분할되었으므로, 던전에서는 앞에서 말한 대로 처리되고, 일반 맵(마을)에서는 quest NPC 가리키도록 처리. quest 받은 경우엔 던전 입구 가리키도록 처리. 
수정안
초기에는 던전을 구역별로 순차 진행하는 구조였기 때문에 각 구역에 배치된 Spawner를 순서대로 목표로 지정했습니다. 플레이어가 Spawner의 Trigger를 밟으면 해당 구역의 전투가 진행되고, 전투가 끝나면 다음Spawner를목표로 지정하는 방식이었습니다.
이후 Quest 시스템이 도입되면서 일반맵과 던전의 진행 방식이 나뉘었습니다. 일반 맵에서는 Quest를진행하기 위해 Quest NPC를 다음 목표로 지정하고 Quest를 받은 이후에는 해당 Quest와 연결된 던전입구를 목표로 지정하도록 변경했습니다. 
던전에서는 기존처럼 Spawner를 순차적으로 진행하도록 처리했습니다.       

Q4. NPC Roaming AI는 어떤 방식으로 구현했나요? 
level에 빈 game object 배치. 캐릭터에 이동 순서대로 할당. NPC Type이 Roaming일 때 현재 지점에서 다음 지점으로 순차 이동. 각 지점마다 idle 여부 결정(random으로 idle or roaming). 플레이어가 말 걸면 멈추고 dialog 진행. 종료 시 다시 roaming 
수정안
Level에 빈 GameObject를 이동 지점으로 배치하고 NPC마다 이동 순서에 맞춰 해당 지점들을 할당했습니다. NPC Type이 Roaming인 경우 현재 지점에서 다음 지점으로 순차적으로 이동하도록 구현했습니다.
각 지점에 도착하면 해당 지점에서 잠시 Idle 상태로 있을지 바로 다음 지점으로 이동할 지를 랜덤으로 결정하도록 했습니다.
플레이어가 NPC에게 대화를 걸면 Roaming을 중단하고 Dialog를 진행하며, 대화가 끝나면 다시 Roaming을 시작하도록 처리했습니다.   

개발 툴
Animation Sequence Editor
Q1. Animation Sequence Editor를 직접 개발하게 된 이유는 무엇이었나요?
대부분의 작업이 그렇겠지만 신입에게 개발 이유가 어딨음? 위에서 시키니까 한 거지. 리소스 최적화 위해서 용량 체크하다가 애니메이션 문제가 있었고, 애니메이션-이벤트 결합 구조라 같은 애니메이션에 다른 이벤트의 경우 애니메이션 양이 늘어남 + 필요 이벤트의 매개변수를 마음대로 할 수 없음. 이걸 해결 위해서 애니메이션 & 이벤트 분리시키고, 애니메이터 작업 편하도록 관련 함수 제공 & 에디터에 적용. 
수정안
처음부터 제가 필요성을 판단해서 새로 만들었다기 보다는 리소스 최적화를 위해 애니메이션 데이터를 확인하는 과정에서 기존 구조의 문제를 발견하게 됐습니다.
기존에는 Animation과 Event가 결합된 구조라서 같은 애니메이션을 사용하더라도 이벤트가 달라지면 애니메이션 데이터가 추가로 필요했고, 이벤트에 필요한 매개변수를 유연하게 지정하기도 어려웠습니다.
그래서 Animation과 Event를 분리하고, Animator가 이벤트를 사용할 수 있도록 관련 함수를 제공한 뒤 이를 Editor에서도 편하게 설정할 수 있도록 구현했습니다.

Q2. Animation과 Event를 분리할 때, 각각 어떤 역할을 담당하도록 설계했나요? 
기존 Unity 파일도 Animation Curve & Event 형식으로 되있음. Animation은 Legacy Animation 사용으로 Play만 시키고, Coroutine을 통해서 현재 Animation의 playtime 확인 후 할당된 Event 메소드 호출 형식. 
수정안
Animation은 Legacy Animation을 이용해 재생 자체만 담당하도록 하고, Event는 Animation과 별도로 관리하도록 분리했습니다.
Animation의 현재 재생 시간을 Coroutine으로 확인하면서 특정 시점에 등록된 Event Method를 호출하는 방식으로 구현했습니다.
이렇게 해서 하나의Animation을 여러 Event 구성에서 재사용할 수 있도록 했습니다. 

Q3. Event를 별도로 관리하기 위해 어떤 데이터 구조를 사용했나요? 
start, end (단발성이 아닌 지속 or 시간 내 움직임 있는 이펙트 있으므로 구간 지정), GameObject, int/float/string params(List) 로 구성된 AniEvent Class. 각 캐릭터는 Dictionary<string, List<AniEvent>> 형태의 sequecnce dictionary갖고 있고, play할 animation 호출 시 해당 dictionary에서 AniEvent List갖고 Coroutine 동작.
수정안
AniEvent라는 별도의 이벤트 데이터를 만들고, Animation 이름을 Key로 하는 Dictionary<string, List<AniEvent>> 형태로 관리했습니다.
AniEvent는 시작 시점과 종료 시점,  대상 GameObject, 그리고 이벤트에 필요한 int, float, string 타입의 매개변수 List를 가지고 있습니다.
시작과 종료 시간을 둘 다 둔 이유는 단발성 이벤트뿐만 아니라 특정 시간 동안 지속되거나 구간 내에서 동작해야 하는 이벤트도 처리하기 위해서였습니다.
캐릭터가 특정 Animation을 재생하면 해당 Animation에 등록된 AniEvent List를 가져오고, Coroutine에서 현재 Animation 재생 시간을 확인하면서 각 Event의 실행 시점을 판단해 등록된 메소드를 호출하는 방식으로 구현했습니다.

Q4. 이 구조를 사용하면서 Animator 작업자가 실제로 이벤트를 등록하고 수정하는 과정은 어떻게 했나요? 
Add button으로 이벤트 추가. 추가 시점은 에디터 상에서 현재 애니메이션 시간. 드래그를 통해 시점 변경 or 시작/종료 시간 변경 가능. 메소드는 한 클래스에 지정. Reflection을 통해 MethodInfo와 ParameterInfo 가져옴. Animator가 메소드 지정(메소드 이름은 Animator가 알기 쉽도록 작성)하면 관련 매개변수 UI 나와서 등록할 수 있도록 함. 이벤트는 당연 삭제도 가능. Save Button누르면 csv 형태(캐릭터이름.csv)로 저장.
수정안
Editor에서는 Add버튼을 통해 현재 Animation 재생 시점에 Event를 추가할 수 있도록 했습니다. 추가된 Event는 Timeline에서드래그하거나 Start와 End 시간을 직접 수정해서 실행 구간을 조정할 수 있도록 했습니다.
Event에서 호출할 메소드는 특정 클래스에 정의하고, Reflection을 이용해 MethodInfo와 Parameterinfo를가져왔습니다. Animator가 메소드를 선택하면 해당 메소드가 요구하는 매개변수에 맞춰 입력 UI가 표시되고, 입력한 값을 Event를 등록할 수 있도록 구현했습니다.
등록된 Event는 필요에 따라 삭제할 수 있고, Save 버튼을 누르면 캐릭터별 csv파일로 저장하도록 구성했습니다.

Q5. Reflection을 사용해 메소드와 파라미터를 자동으로 가져오는 방식의 장점은 무엇이었나요? 
메소드 추가 시 editor쪽 작업 다시 할 필요 없음   
수정안
Reflection을 사용하면 새로운 이벤트 메소드를 추가하더라도 Editor 코드를 다시 수정할 필요가 없다는 장점이 있습니다. 메소드의 MethodInfo와 ParameterInfo를 런타임에 확인해서 필요한 파라미터 UI를 자동으로 구성하기 때문에 이벤트 기능을 추가할 때 Editor를 함께 수정해야 하는 작업을 줄일 수 있었습니다.

Q6. 이 시스템을 실제로 적용하면서 기존 방식보다 어떤 점이 개선되었나요? 
애니메이션 리소스 용량 줄어듦 = 로딩 및 패치 유리. UnityEvent를 통한 제한적 파라미터 등록에서 작업자가 원하는 방식의 대응 가능.
수정안 
기존에는 같은 Animation이라도 Event 구성이 달라지면 Animation 리소스가 추가로 필요했는데, Animation과 Event를 분리하면서 이러한 중복을 줄여 Animation 리소스 용량을 줄일 수 있었습니다. 그 결과 로딩과 패치 측면에서도 유리해졌습니다.
또한 기존 UnityEvent 방식에서는 사용할 수 있는 파라미터가 제한적이었지만, Reflection을 이용해 메소드의파라미터 정보를 가져오도록 하면서 작업자가 필요한 메소드와 파라미터를 보다 유연하게 등록할 수 있도록 개선했습니다. 

Q7. 이 시스템에서 Reflection과 Coroutine을 사용하는 방식의 단점이나 개선할 점은 무엇이라고 생각하나요? 
Editor 작업을 위한 Reflection은 필수. Coroutine도 처음에는 다음 이벤트 시작 시간까지 WaitForSeconds로 했지만 FrameDrop이나 중간에 취소되는 상황 대응이 안 되서 yield return null로 매 프레임 체크로 전환. 아쉬운 점은 이벤트 메소드 호출 방식이 enum인지 functionname(string)인지를 활용한 switch문인데, 이걸 Delegate(Event)로 할당해서 Obsever Pattern을 적용시키면 좀 더 깔끔한 메소드 호출이 가능하지 않을까 싶음.
수정안
Reflection은 Editor에서 메소드와 파라미터를 자동으로 가져오기 위해 필수적이었기 대문에 큰 문제는 없었습니다.
Coroutine은 처음에는 다음 이벤트 시점까지 WaitForSeconds를 사용하는방식으로 구현했지만, Frame Drop이나 Animation이 중간에 취소되는 상황에서 정확한 타이밍제어가 어려웠습니다. 그래서 yield return null 방식으로 매 프레임현재 Animation 시간을 확인하도록 변경해 안정성을 높였습니다.
아쉬운 점은 이벤트 실행 부분이 enum이나 문자열 기반의 switch 구조였다는 점입니다. 다시 구현한다면 Delegate나 Event를 활용해서 Observer Pattern 형태로 구성하면 이벤트 호출 구조를 더 깔끔하고 확장성 있게 만들 수 있다고 생각합니다.    

Q8. CSV 형태로 저장한 이유는 무엇인가요? JSON이나 ScriptableObject 대신 CSV를 선택한 이유는? 
당시엔 ScriptableObject의 존재를 모름. JSON보다 CSV 파싱이 더 간단하다고 생각했음.
수정안
당시에는 Unity의 ScriptableObject를 사용해본 경험이 없었고, 데이터 저장방식으로 CSV를 선택했습니다. CSV는 구조가 단순하고 제가 구현하기에도 파싱이 간단하다고 판단해서 사용했습니다.
현재 다시 구현한다면 ScriptableObject나 JSON같은 다른 데이터 관리 방식도 비교해보고, Unity Editor와의 연동이나 타입 안정성, 유지보수성까지 고려해서 선택할것 같습니다.

Hex Map Editor
Q1. Hex Map Editor를 개발하게 된 배경과 기존 작업 방식의 문제는 무엇이었나요?
Warfront라는 문명과 비슷한 컨텐츠가 있었음. HexMap으로 된 칸(지도) 위에 진영이 있고, 해당 칸마다 랜덤으로 이벤트 발생 & 스토리 진행 or 경쟁 컨텐츠를 통해 지도를 넓혀가는 그런 기획. 문제라기 보다는 이런 Hex Map을 기획자가 제작해야 하는데, 제작 방식이나 툴이 없었기 때문에 작업. (입사할 때 HexMap Editor Prototype - 링크에 있는 내용 바탕으로 HexMap 제작 및 진영 변경 설정 정도가 과제였음)  
수정안
당시 Hex Map을 기반으로 한 콘텐츠가 기획되어 있었습니다. 지도 위의 각칸에 진영이 배치되고, 각 지역에서 랜덤 이벤트나 스토리 진행, 진영 간 경쟁 콘텐츠 등을 통해 영역을 확장하는 형태였습니다.
이 콘텐츠를 제작하려면 기획자가 직접 Hex Map을 구성하고 각 칸의 진영 등을 설정할 수 있어야 했는데, 당시에는 이를 편하게 제작할 수 있는 전용 툴이 없었습니다.
그래서 기획자가 Hex Map을 직접 제작하고 진영을 설정할 수 있도록 Editor를 개발하게 됐습니다. 입사 당시에도 Hex Map Editor Prototype을 개발하는 과제가 있었고, 그 과제에서 요구된 기능을 기반으로 실제 작업으로 확장했습니다. 

Q2. Hex Map Editor에서 기획자가 실제로 어떤 작업을 할 수 있도록 구현했나요? 
초기 Hex Map 작업. 이거는 실제로 Editor에서 Play해서, 각 칸의 진영 선택. 해당 칸의 초기 이벤트 할당. 이거도 csv로 save & load. 나중에는 플레이어 별 로컬저장소에 변경 내용 저장. 나중에서버 연결 및 랜덤 이벤트 작업이 있었던 거 같은데 이건 내가 한 게 아니라서 잘 모르겠음.  
수정안
초기에는 Editor를 Play 상태로 실행한 뒤 Hex Map의 각 칸을 선택해서 진영을 지정하고, 해당 칸에서 시작할 초기 이벤트를 할당할 수 있도록 구현했습니다.
설정한 Map 데이터는 CSV 형태로 Save/Load할 수 있도록 했고, 이후에는 플레이어별 로컬 저장소에 변경된 내용을 저장할 수 있도록 확장했습니다.
이후 서버 연동이나 랜덤 이벤트 관련 기능도 확장된 것으로 알고 있지만, 해당 부분은 제가 직접담당한 영역은 아닙니다. 

Q3. Hex Map의 각 칸을 선택하고 진영을 지정하는 기능은 Editor에서 어떤 방식으로 구현했나요? 
unity의 DrawLine으로 HexMap(Grid) 그림. 맵 크기 설정(ex, 4*3)하면 그 크기만큼 Grid 그림. dropdown으로 진영 추가/삭제해서 설정하고, 그리드 클릭하면 클릭 위치 기반 어떤 hexagon 선택됬는지 확인 후 색깔 변경 및 경계선 작업.
수정안
Unity의 DrawLine을 이용해서 Hex형태의 Grid를 직접 그리도록 구현했습니다. Map 크기를 예를 들어 4 x 3처럼설정하면 해당 크기에 맞춰 Hex Grid가 자동으로 생성되도록 했습니다.
진영은 Dropdown을 통해 추가하거나 삭제하고, Grid를 클릭하면 클릭한 위치를 기반으로 어떤 Hex 선택되었는지 판단하도록 구현했습니다.
선택된 Hex에는 해당 진영의 색상을 적용하고, 진영의 영역을 구분할 수있도록 경계선도 표시했습니다.  

Q4. Hex Map Editor에서 구현하면서 가장 어려웠던 부분은 무엇이었나요? 
경계선. 기획이 짜증났음. 문명을 원함.
수정안
경계선 표현이 어려웠습니다. 처음에는 각지게 만들었지만 자연스럽지 않아 곡선으로 바꾸고 랜덤성을 넣으려고 했습니다. 그런데 인접 영역의 경계가 맞지 않고생성할 때마다 달라지는 문제가 있었습니다. 그래서 실제 지형처럼 표현하려면 HeightMap + Decal 방식이 적합하다고 판단했고, 당시에는 랜덤 공선을 일부 적용하는 수준으로 마무리했습니다. 

Q5. Hex Map Editor를 개발하면서 본인이 가장 크게 얻은 개발 경험은 무엇인가요? 
이론 내용을 실제 구현으로 바꾼 것. 기획에 맞춰서 구현 계속 수정.
수정안
Hex Map과 관련된 이론적인 내용을 실제 Unity Editor 기능으로 구현해보면서, 처음 접한 개념을 실제 시스템으로 적용하는 경험을 했습니다.
또한 처음부터 정해진 형태로 끝나는 것이 아니라실제 기획 의도와 사용성을 확인하면서 구현 방식을 계속 수정하고 보완하는 경험도 할 수 있었습니다. 

최적화
Q1. Animation 리소스 용량을 줄이기 위해 어떤 최적화를 진행했나요? 
Sequence System을 사용한 Animation파일과 Event 분리. Animation 파일 내 Curve 외 불필요한 내용 제거(ex. event) = Animation Compression.
수정안
기존에는 Animation 파일 안에 Animation Curve와Event가 함께 들어가 있었습니다. 
Sequence System을 적용하면서 Animation과 Event를 분리하고, Animation파일에는 실제 애니메이션에 필요한 Curve 정보만 남기도록 했습니다. Event는 별도의 Sequence 데이터로 관리했습니다.
이를 통해 동일한 Animation에 서로 다른 Event가 필요한 경우에도 Animation파일을 추가로 만들 필요가 없어졌고 결과적으로 Animation 리소스 용량을 줄여 로딩이나 패치 측면에서도 유리하도록 개선했습니다.  

Q2. Animation 리소스 용량을 줄이는 작업 외에, 게임 성능 측면에서는 어떤 최적화 경험이 있나요? 
실작업 내용은 아니나 어떤 식으로 최적화 적용했는 지는 알고 있음. 재사용 가능 오브젝트의 경우 Object Pool을 적용하여, 삭제하는 대신 재사용. 작업자에 원칙 세워서, Texture Atlas 구성하도록 설정. 정적인 구조물에 한하여 Light Map Bake. 비용이 많이 들어가는 Pixel Shader나 물리 법칙 적용 Shader(ex. 물, 잔디…)는 가급적 사용 자제. Culling 설정. 정도로 기억.
수정안
직접 구현한 최적화 작업은 아니지만 프로젝트에서 적용된 방식과 목적은 알고 있습니다.
재사용이 많은 Effect나 Projectile같은 오브젝트는 ObjectPool을 사용해서 Instantiate와 Destroy를 반복하지 않고 재사용하는 방식으로 GC와 생성/삭제 비용을 줄입니다.
Texture는 Texture Atlas를 구성해서 여러 텍스처를 하나로 묶고, 이를 통해 Draw Call을 줄이는 방향으로 관리했습니다.
정적인 구조물은 Lightmap을 Bake해서 런타임에 조명 계산을 반복하지 않도록 했고, Pixel Shader나 물/잔디처럼 계산 비용이 큰 Shader는 가급적 사용을 줄였습니다.
그리고 Culling을 적용해서 카메라에 보이지 않는 오브젝트는 랜더링하지 않도록 하는방식으로 랜더링비용을 줄였습니다.   

 

UI / UX
Dialog System
Q1. Dialog System을 개발하게 된 배경과, 기존 대화 시스템에서 어떤 작업을 할 수 있도록 만들었나요?
기존 대화 시스템 없음. 스토리 있는 게임에서는 나레이션 이나 캐릭터 간 대화가 있는 다이얼로그 시스템 필수. 나레이션은 뒷 배경 & 설명 보여주기. 다이얼로그는 뒷배경(없을 수도) 좌/우에 대화하는 캐릭터 일러스트. 대화의 주체가 되는 캐릭터 부각(아마 대화 X 캐릭터는 명도 조절했을 거로 기억) 대화 속도에 맞춰 차례대로 한 글자씩 대화창에 출력. 끝나면 Pause 끝내고 시작.
수정안
기존에 별도의 대화 시스템은 없었고, 스토리가 있는 게임에서는 나레이션이나 캐릭터 간 대화를 표현하기 위한 Dialog System이 필요하다고 판단되어 개발했습니다.
나레이션에서는 배경과 설명을 보여주고, 캐릭터 대화에서는 배경과 좌우에 배치된 캐릭터 일러스트를 표시하도록 했습니다. 현재 대화하는 캐릭터를 강조하고, 대화하지 않는 캐릭터는 명도를 낮춰 대화의 주체를구분할 수 있도록 했습니다.
대사는 설정된 대화 속도에 맞춰한 글자씩 출력하도록 했고, Dialog가 시작되면 게임을 일시정지하고 Dialog가 종료되면 다시 게임을 진행하도록 구현했습니다.

  Q2. Dialog System에서 대사를 한 글자씩 출력하도록 구현할 때, 어떤 부분을 고려해야 했나요? 
Coroutine으로 상태 진입 게임 상태 Dialog면 Game Pause. ui.text에 string for문 돌리면서 속도에 맞춰 wait for seconds하면서 한글자씩 더하기. dialog list 순회 다 하면 dialog 끝 판단 coroutine 끝냄. 
수정안
Dialog 상태에 진입하면 Coroutine을 실행하면서 게임을 Pause합니다.
대화 목록을 순회하면서 각 대사의 문자열을 for문으로 한글자씩 UI.text에 추가하고, 설정된 대화 속도만큼 WaitForSeconds로 대기하는 방식으로 구현했습니다.
모든 대화를 순회하면 Dialog가 종료된 것으로 판단하고 Coroutine을 끝낸 뒤 게임을 다시 진행하도록 했습니다.

Q3. Dialog가 종료됐을 때 게임을 다시 진행시키는 처리는 어떻게 구현했나요? 
GameState가 Dialog면 AI는 idle 전환. 끝나면 GameState Playing으로 전환해서 기존의 AI 로직 진행. 정확한 GameState 명이 저랬는지는 기억 잘 안남.
수정안
정확한 상태 이름은 기억나지 않지만, Dialog 상태에 진입하면 게임 상태를 Dialog 상태로전환하고 AI는 Idle 상태로 전환하도록 했습니다.
Dialog가 종료되면 게임 상태를 다시 Playing 상태로 전환하고, AI도 기존 게임 로직에 따라 다시 동작하도록 했습니다. 

Q4. Dialog System에서 나레이션과 캐릭터 대화를 하나의 시스템에서 처리하기 위해 어떻게 구분했나요? 
같은 UI Base 상속 받아서 나레이션과 다이얼로그 enum 타입 변환 및 내부 구현 다르게 
수정안
나레이션과 캐릭터 대화는 같은 UI Base를 상속받도록 구성하고, Dialog 타입을 enum으로 구분했습니다. 
공통적인 UI 처리 부분은 Base에서 관리하고, 실제 표시 방식은 타입에 따라 다르게 구현했습니다. 나레이션이면 배경과 설명을 표시하고, 캐릭터 대화라면 캐릭터 일러스트와 대사 등을 표시하는 방식입니다.

Q5. Dialog System에서 대화 속도나 대사 진행을 처리하면서 고려한 부분이 있었나요? 
처음엔 한번에 보이게 했었는데 기획 요청으로 대화 속도에 따라 대사 진행 처리. 실제 녹음본이 없기 때문에 속도는 사람이 편하게 읽기 쉬운 정도의 속도로 고정. (기획이 속도 조정 가능하도록 했는지는 기억 안남. 이것도 결국 대화 자료가 있고, 로드해서 사용했는데 나중에는 서버 연결로 가져왔던 거 같기도 함.) 다만 플레이어를 위해 다이얼로그 창 한 번 누르면 나머지 텍스트 다 표시 & 다음 버튼 출현, 그리고 상단에 스킵 버튼으로 다이얼로그 자체를 스킵하는 기능은 넣어둠.(UI 기획자가 작업한 prefab 기준으로 기능 구현) 
수정안
처음에는 대사가 한 번에 표시되도록 구현했지만 기획 요청으로 대화 속도에 맞춰 한 글자씩 출력하도록 변경했습니다. 당시에는 실제 음성 녹음본이 없었기 때문에 사람이 읽었을 때 자연스럽게 느껴지는 정도의 속도로 설정했습니다.
그리고 플레이어가 대사를 기다리는 것이 불편할 수 있기 때문에 대화창을 한 번 누르면 현재 대사의 남은 텍스트를 즉시 표시하고 다음대사로 넘어갈 수 있도록 했습니다. 또한 상단에 스킵 버튼을 추가해 Dialog 자체를 건너뛸 수 있도록 구현했습니다.
UI 자체는 UI 기획자가 제작한 Prefab을 기준으로 기능을 구현했습니다. 

서버 연동


공통 검증

