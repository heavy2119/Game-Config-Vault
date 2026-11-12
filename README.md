# C.W Config Copy

게임과 프로그램의 설정 파일을 복사해 두고, 원하는 시점으로 되돌리는 Windows 프로그램입니다.

설정 폴더를 통째로 다시 넣지 않습니다. 마지막으로 복사한 내용과 비교해서, 새로 생겼거나 바뀌거나 지워진 파일만 새 ZIP에 담습니다. 같은 파일은 이전 ZIP을 그대로 가리킵니다. ZIP은 Windows 탐색기에서 열 수 있습니다.

---

## 하는 일

- 게임을 고르면 그 게임의 기본 설정 폴더를 백업 항목으로 넣습니다. 폴더는 나중에 바꿀 수 있습니다.
- 폴더나 파일을 직접 추가할 수도 있습니다.
- 백업하지 않을 파일을 규칙으로 뺍니다. 처음에는 `desktop.ini`, `Thumbs.db`, 확장자 `tmp`가 빠져 있습니다.
- 기록에서 그 시점의 설정을 게임 폴더에 다시 넣습니다. 고른 파일만 넣을 수도 있습니다.
- 예전에 만들어 둔 ZIP을 기록으로 불러옵니다. 날짜는 ZIP에 저장된 날짜를 그대로 씁니다.
- 기록 항목을 지우면, 다른 기록이 쓰지 않는 ZIP 파일도 같이 지웁니다.
- 두 시점, 또는 저장본과 현재 폴더를 비교합니다.
- Windows 작업 스케줄러로 정해 둔 시각에 복사합니다. 프로그램 창이 꺼져 있어도 실행됩니다. 바뀐 파일이 없으면 새 기록은 만들지 않습니다.

기본으로 들어 있는 대상은 Assetto Corsa, Assetto Corsa Competizione, Assetto Corsa Rally, Assetto Corsa EVO, Le Mans Ultimate, Automobilista 2, Project Motor Racing, SimHub 입니다. 그 외 프로그램은 폴더를 직접 추가하면 됩니다.

## 화면

| 메뉴 | 내용 |
| --- | --- |
| 현황 | 보관 디스크, 게임 수, 각 게임의 마지막 복사 |
| 게임 | 게임 추가, 백업 폴더, 규칙, 백업 |
| 기록 | 되돌리기, ZIP 불러오기, 항목 삭제, 메모 |
| 비교 | 저장본끼리, 또는 저장본과 현재 폴더 |
| 설정 | 모든 게임에 적용되는 규칙, 무결성 검사, 예약 |

## 필요 환경

- Windows 10 또는 Windows 11, 64비트
- [.NET 10 데스크톱 런타임](https://dotnet.microsoft.com/download/dotnet/10.0)

프로그램을 빌드하려면 .NET 10 SDK도 필요합니다. 이 저장소의 `global.json`은 SDK 10.0.403을 가리키며, 같은 기능 대역의 더 새 SDK도 사용할 수 있습니다.

보관 위치는 드라이브 문자가 아니라 디스크 고유 번호로 기억합니다. 문자만 바뀌어도 같은 디스크면 이어서 엽니다. 고유 번호를 알 수 없는 경로는 보관 위치로 쓸 수 없습니다.

## 빌드와 실행

```powershell
dotnet build ConfigVault.slnx
dotnet run --project src\ConfigVault.App\ConfigVault.App.csproj
```

실행 파일은 `src\ConfigVault.App\bin\Debug\net10.0-windows\ConfigVault.exe` 입니다.

예약은 같은 폴더의 `ConfigVault.Cli.exe`를 작업 스케줄러가 실행합니다. 이 파일은 앱을 빌드할 때 함께 복사됩니다. 배포할 때는 `ConfigVault.exe`만 옮기지 말고, 그 폴더에 함께 있는 프로그램 파일과 `runtimes\win-x64`를 같이 두어야 합니다. 리눅스, macOS용 파일은 필요 없습니다.

## 예약

설정에서 시각을 정하고 예약 등록을 누르면 Windows 작업 스케줄러에 작업이 생깁니다.

- `daily`: 매일 그 시각
- `weekly`: 매주 월요일 그 시각
- `login`: Windows에 로그인할 때. 시각은 쓰지 않습니다
- `clock`: 그 시각에 한 번

실행되면 명령창에 진행이 나오고, 끝나면 창이 닫힙니다. 컴퓨터가 꺼져 있거나 잠자기 상태여서 그 시각을 놓치면, 나중에 자동으로 이어서 실행하지는 않습니다.

## 테스트

```powershell
dotnet test ConfigVault.slnx
```

---

# C.W Config Copy

A Windows app that keeps copies of game and program settings and puts a chosen copy back.

It does not recopy a settings folder from scratch. It compares the folder with the last copy and stores only files that were added, changed, or removed. Unchanged files stay pointed at the earlier ZIP. The archives are normal ZIP files and open in File Explorer.

## What it does

- Adding a game starts from that game's usual settings folders. Those folders can be changed later.
- You can also add any folder or file yourself.
- Rules leave files out of a backup. New vaults skip `desktop.ini`, `Thumbs.db`, and the `tmp` extension.
- History can put that copy back into the game folder, or put back only the files you select.
- An existing ZIP can be loaded into History. The date stored in the ZIP is kept.
- Deleting a history entry also deletes its ZIP when no other entry still needs that file.
- Two saved copies can be compared, or a saved copy can be compared with a folder on disk.
- Windows Task Scheduler can run a copy at a set time while the main window is closed. If nothing changed, no new history entry is created.

Built-in profiles cover Assetto Corsa, Assetto Corsa Competizione, Assetto Corsa Rally, Assetto Corsa EVO, Le Mans Ultimate, Automobilista 2, Project Motor Racing, and SimHub. Anything else is added by choosing its folder.

## Screens

| Page | What it is |
| --- | --- |
| Status | Vault disk, game count, and each game's latest copy |
| Games | Add a game, choose folders, edit rules, run a backup |
| History | Restore, import a ZIP, delete an entry, edit a note |
| Compare | Two saved copies, or a saved copy and a folder |
| Settings | Rules that apply to every game, integrity check, schedule |

## Requirements

- 64-bit Windows 10 or Windows 11
- [.NET 10 Desktop Runtime](https://dotnet.microsoft.com/download/dotnet/10.0)

Building also needs the .NET 10 SDK. `global.json` pins SDK 10.0.403 and allows a newer SDK in the same feature band.

The vault is remembered by the disk's volume id, not by the drive letter. The same disk still opens if the letter changes. A path with no volume id cannot be used as the vault.

## Build and run

```powershell
dotnet build ConfigVault.slnx
dotnet run --project src\ConfigVault.App\ConfigVault.App.csproj
```

The app is `src\ConfigVault.App\bin\Debug\net10.0-windows\ConfigVault.exe`.

Scheduled copies run `ConfigVault.Cli.exe` from that same folder. The app build copies the CLI there. When you share the app, keep the files next to `ConfigVault.exe`, including `runtimes\win-x64`. Linux and macOS files in `runtimes` are not needed.

## Schedule

Registering a schedule creates a Windows Task Scheduler task.

- `daily`: every day at the chosen time
- `weekly`: every Monday at the chosen time
- `login`: when you sign in to Windows. The time is ignored
- `clock`: once, at the chosen time

A console window shows progress and closes when the copy finishes. If the PC is off or asleep at that time, the missed copy is not run later.

## Tests

```powershell
dotnet test ConfigVault.slnx
```
