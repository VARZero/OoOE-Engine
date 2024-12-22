# Out-of-Order Execution Engine(OoOE Engine)
비순차적 실행 처리는 CPU에서 Instruction의 처리를 진행할 때 최종적으로 입력된 순서대로 진행되지만, 내부에서 순서를 바꿔 처리하는 시스템을 말합니다. 이는 Clock cycles Per Instruction(CPI)를 증가시키는 방법론 중 하나입니다.
## 사용되는 이론
- Register Renaming: 논리 레지스터를 가상 레지스터 번호로 생각하고, 물리 레지스터에 매핑하여 데이터 해저드를 방지하는 이론입니다.
- Tomasulo Algorithm: Register Renaming이 진행된 Instruction이 분배되는 되는 방법입니다.

## Components
OoOE 엔진에는 여러가지 요소가 있습니다.

### Reservation Station
데이터 분배 처리를 위한 버퍼입니다.
#### Fields
- Busy: 사용되고 있는 중인지 나타내는 Indicater입니다.
- Opcode: 수행할 연산의 Opcode입니다.
- Vj, Vk: 준비되어야 하는 값입니다.
- Qj, Qk: 준비되어야 하는 레지스터 번호나 ROB 번호입니다.
- Destination: 결과가 저장될 레지스터 번호입니다.
- Ready: Qj와 Qk가 준비되었는지 나타내는 Indicater입니다.

### Reorder Buffer(ROB)
연산의 결과가 저장되는 버퍼입니다.
#### Fields
- Busy: 사용되고 있는 중인지 나타내는 Indicater입니다.
- Opcode: 수행할 연산의 Opcode입니다.
- Destination: 결과가 저장될 레지스터 번호입니다.
- Value: 결과 값입니다.
- Ready: ROB의 값이 준비되었는지 나타내는 Indicater입니다.

## Assume
한번 구현해보기 위해 여러 가정을 만들어 보았습니다.
### ISA
https://github.com/VARZero/MakeModCpu16
### 물리 레지스터 수
논리 레지스터 수의 4배인 64개로 지정합니다.
### Reservation Station
FIFO 메모리로 구현하고, 개당 24개 입니다.
### Reorder Buffer
FIFO 메모리로 구현하고, Entry 수는 64개 입니다.