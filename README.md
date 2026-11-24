# 64×64 武俠 2.5D 角色產生器：LLM / GitHub Pages 使用說明

這是一個完全前端的單檔 HTML 角色產生器。放到 GitHub Pages 後，LLM 或自動化腳本只需要組合 URL query string，就能還原角色屬性並直接下載 PNG，不需要後端服務或 API key。

## URL 產生角色

基本格式：

```text
https://kaojarher.github.io/GameRoleGen/index.html?gender=female&class=mage&ears=elf&hair=ponytail&weapon=staff&cape=1&beard=0&skin=%23f2c29b&hairColor=%232c1e16&primary=%232a3f8f&accent=%23e0a82e&capeColor=%232b5fb0&bg=transparent&zoom=4
```

可用參數：

| 參數 | 可用值 |
|---|---|
| `gender` | `male`, `female` |
| `class` | `noble`, `elder`, `swordsman`, `general`, `mystic`, `mage`, `priest`, `rogue`, `ranger`, `monk` |
| `hair` | `crown`, `ponytail`, `elderBun`, `twinBun`, `headband` |
| `ears` | `human`, `elf`, `beast`, `demon` |
| `weapon` | `none`, `backSword`, `whisk`, `broadsword`, `staff` |
| `cape` | `0`, `1` |
| `beard` | `0`, `1` |
| `skin` | 六碼色碼，例如 `%23f2c29b` |
| `hairColor` | 六碼色碼，例如 `%232c1e16` |
| `primary` | 六碼色碼，例如 `%232a3f8f` |
| `accent` | 六碼色碼，例如 `%23e0a82e` |
| `capeColor` | 六碼色碼，例如 `%232b5fb0` |
| `bg` | `green`, `transparent` |
| `zoom` | `2`, `4`, `6` |

## URL 直接下載 PNG

在角色 URL 後加入以下任一參數，頁面完成渲染後會自動下載：

```text
&download=front   # 64×64 45°正面 PNG
&download=back    # 64×64 45°背面 PNG
&download=sheet   # 128×64 正背合併 PNG
```

例如：

```text
https://YOUR_GITHUB_USER.github.io/YOUR_REPO/64_64%E6%AD%A3%E9%9D%A2.html?gender=female&class=general&hair=ponytail&weapon=broadsword&download=front
```

注意：瀏覽器可能會要求使用者允許該網站下載檔案；這是瀏覽器的下載安全政策，不是產生器限制。

## JavaScript API

如果 LLM 或瀏覽器自動化已經開啟該頁面，可使用 `window.CharacterGenerator`：

```javascript
CharacterGenerator.setState({
  gender: 'female',
  cls: 'mage',
  ears: 'elf',
  hair: 'ponytail',
  weapon: 'staff',
  cape: true,
  beard: false,
  colSkin: '#f2c29b',
  colHair: '#2c1e16',
  colPrimary: '#2a3f8f',
  colAccent: '#e0a82e',
  colCape: '#2b5fb0'
});

const state = CharacterGenerator.getState();
const shareUrl = CharacterGenerator.getUrl();
CharacterGenerator.download('front'); // front / back / sheet
```

## 給 LLM 的建議流程

1. 先將自然語言需求轉換成固定 enum，例如「女俠、馬尾、佩刀、藍色服裝」轉成 `gender=female&class=swordsman&hair=ponytail&weapon=broadsword&primary=%232a3f8f`。
2. 所有 `#` 色碼必須 URL encode 成 `%23`。
3. 產生 GitHub Pages URL。
4. 若需要檔案，加入 `download=front`、`download=back` 或 `download=sheet`。
5. 不要自行修改 enum 名稱；無效值會被頁面安全忽略或回到預設值。

建議 LLM 輸出格式：

```json
{
  "url": "https://YOUR_GITHUB_USER.github.io/YOUR_REPO/64_64%E6%AD%A3%E9%9D%A2.html?...",
  "download": "front",
  "attributes": {
    "gender": "female",
    "class": "swordsman",
    "hair": "ponytail",
    "ears": "elf",
    "weapon": "broadsword"
  }
}
```
