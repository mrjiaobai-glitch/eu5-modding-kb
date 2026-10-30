# 参考资料：GUI 数据类型速查（data types）

> **用途**：写 GUI / 本地化表达式时查数据函数（`GetXxx`）、全局 promote（`COUNTRY` 这类）、返回类型与说明。
> **来源**：游戏内 `dump_data_types` 生成的 `logs\data_types\`，**EU5 1.4.5**，2026-10-30。
> **体量**：**检索用清单**；本档收录 3 档（gui 1148 / script 956 / common 345 条）。

## 目录

- [data_types_gui.txt（1148 条）](#data_types_guitxt1148-条)
- [data_types_script.txt（956 条）](#data_types_scripttxt956-条)
- [data_types_common.txt（345 条）](#data_types_commontxt345-条)

| 档 | 条目数 | 说明 |
|---|---|---|
| `data_types_gui.txt` | 1148 | GUI 侧可调用函数与 GUI 类型 |
| `data_types_script.txt` | 956 | 脚本侧常量与全局（DATE、COUNTRY 等） |
| `data_types_common.txt` | 345 | 公共 promote / 系统入口 |
| `data_types_internalclausewitzgui.txt` | 3,503 | 引擎内部 GUI（开发工具用，未收录） |
| `data_types_uncategorized.txt` | 25,021 | 未分类（体量过大，未收录；需要时直接 grep 原档） |

## data_types_gui.txt（1148 条）

| 名称 | 返回类型 | 定义类型 | 说明 |
|---|---|---|---|
| `AccessModsGui` | ModsGui | Global promote | Access Mods GUI |
| `AccessModsGui` | [unregistered] | Global function | Access Mods GUI |
| `AccountInventory` |  | Type |  |
| `AccountInventory.AccessSelf` | [unregistered] | Function |  |
| `AccountInventory.HasItem( Arg0 )` | bool | Function |  |
| `AccountInventory.HasRetrievedInventory` | bool | Function |  |
| `AccountInventory.Self` | [unregistered] | Function |  |
| `Achievement` |  | Type | Jomini Achievement |
| `Achievement.AccessSelf` | [unregistered] | Function |  |
| `Achievement.IsUnlocked` | bool | Function |  |
| `Achievement.Self` | [unregistered] | Function |  |
| `AchievementPopup` |  | Type |  |
| `AchievementPopup.AccessJustUnlockedAchievements` | [unregistered] | Function |  |
| `AchievementPopup.AccessSelf` | [unregistered] | Function |  |
| `AchievementPopup.Self` | [unregistered] | Function |  |
| `AchievementPopup.ShowWindow` | void | Function |  |
| `AchievementWindow` |  | Type | Jomini Achievement Window |
| `AchievementWindow.AccessGroups` | [unregistered] | Function |  |
| `AchievementWindow.AccessSelf` | [unregistered] | Function |  |
| `AchievementWindow.HasGameStarted` | bool | Function |  |
| `AchievementWindow.Hide` | void | Function |  |
| `AchievementWindow.Self` | [unregistered] | Function |  |
| `AchievementWindow.ShouldShowAll` | bool | Function |  |
| `AchievementWindow.ShouldShowLocked` | bool | Function |  |
| `AchievementWindow.ShouldShowPossible` | bool | Function |  |
| `AchievementWindow.ShouldShowUnlocked` | bool | Function |  |
| `AchievementWindow.Show` | void | Function |  |
| `AchievementWindow.ShowAll` | void | Function |  |
| `AchievementWindow.ShowLocked` | void | Function |  |
| `AchievementWindow.ShowPossible` | void | Function |  |
| `AchievementWindow.ShowUnlocked` | void | Function |  |
| `AddFriendWindow` |  | Type | Jomini Friends - Add Friends Window |
| `AddFriendWindow.AccessSelf` | [unregistered] | Function |  |
| `AddFriendWindow.Hide` | void | Function |  |
| `AddFriendWindow.Self` | [unregistered] | Function |  |
| `AddFriendWindow.ShouldBeShown` | bool | Function |  |
| `AddFriendWindow.Show` | void | Function |  |
| `AvailabilityEntry` |  | Type | Jomini Friend List - Availibility Entry |
| `AvailabilityEntry.AccessSelf` | [unregistered] | Function |  |
| `AvailabilityEntry.GetAvailabilityString` | CUTF8String | Function |  |
| `AvailabilityEntry.Self` | [unregistered] | Function |  |
| `BindFoldOutContext` | void | Global function | Add a new PdxGuiFoldOut data context to a widget. |
| `CanEditSettingsAfterHost` | bool | Global function | Multiplayer - If you can edit settings after hosting (depends on MP platform) |
| `Chat` |  | Type | Jomini Chat - Chat |
| `Chat.AccessChatMessages` | [unregistered] | Function |  |
| `Chat.AccessSelf` | [unregistered] | Function |  |
| `Chat.GetReceiver` | [unregistered] | Function |  |
| `Chat.GetReceiver` | Friend | Promote |  |
| `Chat.GetReceiverName` | CUTF8String | Function |  |
| `Chat.GetUnreadMessageCount` | int32 | Function |  |
| `Chat.HasUnreadMessages` | bool | Function |  |
| `Chat.IsRoomChat` | bool | Function |  |
| `Chat.Self` | [unregistered] | Function |  |
| `ChatMessage` |  | Type | Jomini Chat - Message |
| `ChatMessage.AccessSelf` | [unregistered] | Function |  |
| `ChatMessage.GetMessage` | CUTF8String | Function |  |
| `ChatMessage.GetName` | CUTF8String | Function |  |
| `ChatMessage.Self` | [unregistered] | Function |  |
| `ChatNotificationMessage` |  | Type | Jomini Chat - Notification message |
| `ChatNotificationMessage.AccessSelf` | [unregistered] | Function |  |
| `ChatNotificationMessage.ChatMessage` | ChatMessage | Promote |  |
| `ChatNotificationMessage.ChatMessage` | [unregistered] | Function |  |
| `ChatNotificationMessage.Kill` | void | Function |  |
| `ChatNotificationMessage.Self` | [unregistered] | Function |  |
| `ChatNotificationMessage.StartChat` | void | Function |  |
| `ChatTab` |  | Type | Jomini Chat - Tab |
| `ChatTab.AccessSelf` | [unregistered] | Function |  |
| `ChatTab.Activate` | void | Function |  |
| `ChatTab.Close` | void | Function |  |
| `ChatTab.GetChat` | [unregistered] | Function |  |
| `ChatTab.GetChat` | Chat | Promote |  |
| `ChatTab.GetChatTitle` | CUTF8String | Function |  |
| `ChatTab.IsActive` | bool | Function |  |
| `ChatTab.Self` | [unregistered] | Function |  |
| `ChatWindow` |  | Type | Jomini Chat - Window |
| `ChatWindow.AccessChatTabs` | [unregistered] | Function |  |
| `ChatWindow.AccessSelf` | [unregistered] | Function |  |
| `ChatWindow.CanSendMessage` | bool | Function |  |
| `ChatWindow.Hide` | void | Function |  |
| `ChatWindow.Self` | [unregistered] | Function |  |
| `ChatWindow.SendMessage` | void | Function |  |
| `ChatWindow.ShouldBeShown` | bool | Function |  |
| `ChatWindow.Show` | void | Function |  |
| `ClearHostError` | void | Global function | Multiplayer - Clear host error |
| `CoatOfArms` |  | Type | Coat of Arms Sprite |
| `CoatOfArms.AccessSelf` | [unregistered] | Function |  |
| `CoatOfArms.GetColor( Arg0 )` | CVector4f | Function |  |
| `CoatOfArms.GetFrame( Arg0, Arg1 )` | int32 | Function |  |
| `CoatOfArms.GetSlot( Arg0, Arg1 )` | CVector4f | Function |  |
| `CoatOfArms.GetTexture( Arg0, Arg1 )` | [unregistered] | Function |  |
| `CoatOfArms.Self` | [unregistered] | Function |  |
| `CoatOfArmsWrapper` |  | Type | Coat of Arms Sprite Wrapper |
| `CoatOfArmsWrapper.AccessSelf` | [unregistered] | Function |  |
| `CoatOfArmsWrapper.GetColor( Arg0 )` | CVector4f | Function |  |
| `CoatOfArmsWrapper.GetFrame( Arg0, Arg1 )` | int32 | Function |  |
| `CoatOfArmsWrapper.GetFrameSizeOrFallback( Arg0, Arg1, Arg2 )` | CVector2i | Function |  |
| `CoatOfArmsWrapper.GetSlot( Arg0, Arg1 )` | CVector4f | Function |  |
| `CoatOfArmsWrapper.GetTexture( Arg0, Arg1 )` | [unregistered] | Function |  |
| `CoatOfArmsWrapper.Self` | [unregistered] | Function |  |
| `CopyServerID` | void | Global function | Multiplayer - Copy Server ID of current session to clipboard |
| `CountryEntry` |  | Type | Country Entry |
| `CountryEntry.AccessSelf` | [unregistered] | Function |  |
| `CountryEntry.GetCountry` | CUTF8String | Function |  |
| `CountryEntry.Self` | [unregistered] | Function |  |
| `CPdxInputBindingSetting` |  | Type | Jomini Settings - Rebindable Input Binding Setting |
| `CPdxInputBindingSetting.AccessSelf` | [unregistered] | Function |  |
| `CPdxInputBindingSetting.Self` | [unregistered] | Function |  |
| `CreateAccount` |  | Type | Create Account View |
| `CreateAccount.AccessSelf` | [unregistered] | Function |  |
| `CreateAccount.CanCreateAccount` | bool | Function |  |
| `CreateAccount.CreateAccount` | void | Function |  |
| `CreateAccount.GetCountries` | [unregistered] | Function |  |
| `CreateAccount.GetDays` | [unregistered] | Function |  |
| `CreateAccount.GetEmail` | CUTF8String | Function |  |
| `CreateAccount.GetErrorMessage` | CUTF8String | Function |  |
| `CreateAccount.GetMonths` | [unregistered] | Function |  |
| `CreateAccount.GetOptedIntoMarketingChecked` | bool | Function |  |
| `CreateAccount.GetPassword` | CUTF8String | Function |  |
| `CreateAccount.GetSelectedCountryIndex` | CVector2i | Function |  |
| `CreateAccount.GetSelectedDayIndex` | CVector2i | Function |  |
| `CreateAccount.GetSelectedMonthIndex` | CVector2i | Function |  |
| `CreateAccount.GetSelectedYearIndex` | CVector2i | Function |  |
| `CreateAccount.GetYears` | [unregistered] | Function |  |
| `CreateAccount.Hide` | void | Function |  |
| `CreateAccount.IsCreatingAccount` | bool | Function |  |
| `CreateAccount.OnCountryChanged` | void | Function |  |
| `CreateAccount.OnDayChanged` | void | Function |  |
| `CreateAccount.OnMonthChanged` | void | Function |  |
| `CreateAccount.OnOptedIntoMarketingToggled` | void | Function |  |
| `CreateAccount.OnYearChanged` | void | Function |  |
| `CreateAccount.Self` | [unregistered] | Function |  |
| `CreateAccount.SetEmail` | void | Function |  |
| `CreateAccount.SetPassword` | void | Function |  |
| `CreateAccount.Show` | void | Function |  |
| `CreateSocialProfileWindow` |  | Type | Jomini Social Profile - Create window |
| `CreateSocialProfileWindow.AccessSelf` | [unregistered] | Function |  |
| `CreateSocialProfileWindow.Create` | void | Function |  |
| `CreateSocialProfileWindow.GetErrorMessage` | CString | Function |  |
| `CreateSocialProfileWindow.GetName` | CString | Function |  |
| `CreateSocialProfileWindow.Hide` | void | Function |  |
| `CreateSocialProfileWindow.IsCreatingProfile` | bool | Function |  |
| `CreateSocialProfileWindow.IsValidName` | bool | Function |  |
| `CreateSocialProfileWindow.Self` | [unregistered] | Function |  |
| `CreateSocialProfileWindow.SetName` | void | Function |  |
| `CreateSocialProfileWindow.ShouldBeShown` | bool | Function |  |
| `CreateSocialProfileWindow.Show` | void | Function |  |
| `CreditsWindow` |  | Type | Jomini Credits Window |
| `CreditsWindow.AccessSelf` | [unregistered] | Function |  |
| `CreditsWindow.GetCredits` | CUTF8String | Function |  |
| `CreditsWindow.HasReachedEnd` | bool | Function |  |
| `CreditsWindow.IsFastest` | bool | Function |  |
| `CreditsWindow.IsPaused` | bool | Function |  |
| `CreditsWindow.IsSlowest` | bool | Function |  |
| `CreditsWindow.OnClose` | void | Function |  |
| `CreditsWindow.OnFaster` | void | Function |  |
| `CreditsWindow.OnSlower` | void | Function |  |
| `CreditsWindow.OnTogglePause` | void | Function |  |
| `CreditsWindow.Self` | [unregistered] | Function |  |
| `EditorSettingCategory` |  | Type | Editor Settings - Category |
| `EditorSettingCategory.AccessSelf` | [unregistered] | Function |  |
| `EditorSettingCategory.GetName` | CString | Function |  |
| `EditorSettingCategory.Self` | [unregistered] | Function |  |
| `EditorSettingsPage` |  | Type | Editor Settings - Settings Page |
| `EditorSettingsPage.AccessCategories` | [unregistered] | Function |  |
| `EditorSettingsPage.AccessSelf` | [unregistered] | Function |  |
| `EditorSettingsPage.GetName` | CString | Function |  |
| `EditorSettingsPage.IsSelected` | bool | Function |  |
| `EditorSettingsPage.OnClick` | void | Function |  |
| `EditorSettingsPage.Self` | [unregistered] | Function |  |
| `EditorSettingsWindow` |  | Type | Editor Settings Window |
| `EditorSettingsWindow.AccessActivePage` | [unregistered] | Function |  |
| `EditorSettingsWindow.AccessActivePage` | EditorSettingsPage | Promote |  |
| `EditorSettingsWindow.AccessPages` | [unregistered] | Function |  |
| `EditorSettingsWindow.AccessSelf` | [unregistered] | Function |  |
| `EditorSettingsWindow.AllowApply` | bool | Function |  |
| `EditorSettingsWindow.AppliedOnRestart` | bool | Function |  |
| `EditorSettingsWindow.Close` | void | Function |  |
| `EditorSettingsWindow.GetWindowName` | CString | Function |  |
| `EditorSettingsWindow.HasChanged` | bool | Function |  |
| `EditorSettingsWindow.Hide` | void | Function |  |
| `EditorSettingsWindow.OnApply` | void | Function |  |
| `EditorSettingsWindow.RequireRestart` | bool | Function |  |
| `EditorSettingsWindow.Restore` | void | Function |  |
| `EditorSettingsWindow.Save` | void | Function |  |
| `EditorSettingsWindow.SaveAndClose` | void | Function |  |
| `EditorSettingsWindow.Self` | [unregistered] | Function |  |
| `EditorSettingsWindow.Show` | void | Function |  |
| `Encyclopedia` |  | Type | Jomini Encyclopedia |
| `Encyclopedia.AccessCurrentPage` | EncyclopediaPage | Promote |  |
| `Encyclopedia.AccessCurrentPage` | [unregistered] | Function |  |
| `Encyclopedia.AccessPages` | [unregistered] | Function |  |
| `Encyclopedia.AccessSelf` | [unregistered] | Function |  |
| `Encyclopedia.CanFilterFromHistoryBack` | bool | Function |  |
| `Encyclopedia.CanFilterFromHistoryForward` | bool | Function |  |
| `Encyclopedia.FilterFromHistoryBack` | void | Function |  |
| `Encyclopedia.FilterFromHistoryForward` | void | Function |  |
| `Encyclopedia.GetAllPage` | EncyclopediaPage | Promote |  |
| `Encyclopedia.GetAllPage` | [unregistered] | Function |  |
| `Encyclopedia.GetCurrentPage` | EncyclopediaPage | Promote |  |
| `Encyclopedia.GetCurrentPage` | [unregistered] | Function |  |
| `Encyclopedia.GetPages` | [unregistered] | Function |  |
| `Encyclopedia.GetViewedEntries` | [unregistered] | Function |  |
| `Encyclopedia.Hide` | void | Function |  |
| `Encyclopedia.IsShown` | bool | Function |  |
| `Encyclopedia.Self` | [unregistered] | Function |  |
| `Encyclopedia.SetCurrentPage( Arg0 )` | void | Function |  |
| `Encyclopedia.ViewAllEntries` | void | Function |  |
| `EncyclopediaEntry` |  | Type | Jomini Encyclopedia - Entry |
| `EncyclopediaEntry.AccessSelf` | [unregistered] | Function |  |
| `EncyclopediaEntry.GetBody` | CString | Function |  |
| `EncyclopediaEntry.GetFrame` | int32 | Function |  |
| `EncyclopediaEntry.GetFrameSize` | CVector2i | Function |  |
| `EncyclopediaEntry.GetImage` | [unregistered] | Function |  |
| `EncyclopediaEntry.GetTitle` | CString | Function |  |
| `EncyclopediaEntry.HasImage` | bool | Function |  |
| `EncyclopediaEntry.Self` | [unregistered] | Function |  |
| `EncyclopediaEntryView` |  | Type | Jomini Encyclopedia - Entry View |
| `EncyclopediaEntryView.AccessSelf` | [unregistered] | Function |  |
| `EncyclopediaEntryView.Get` | [unregistered] | Function |  |
| `EncyclopediaEntryView.Get` | EncyclopediaEntry | Promote |  |
| `EncyclopediaEntryView.Self` | [unregistered] | Function |  |
| `EncyclopediaPage` |  | Type | Jomini Encyclopedia - Page |
| `EncyclopediaPage.AccessSelf` | [unregistered] | Function |  |
| `EncyclopediaPage.FilterFromText` | void | Function |  |
| `EncyclopediaPage.GetTitle` | CString | Function |  |
| `EncyclopediaPage.ResetFilter` | void | Function |  |
| `EncyclopediaPage.SearchFromEnteredText` | void | Function |  |
| `EncyclopediaPage.Self` | [unregistered] | Function |  |
| `EndPrepConfirm` |  | Type | Jomini Multiplayer - End preparation confirm |
| `EndPrepConfirm.AccessSelf` | [unregistered] | Function |  |
| `EndPrepConfirm.Cancel` | void | Function |  |
| `EndPrepConfirm.Confirm` | void | Function |  |
| `EndPrepConfirm.Self` | [unregistered] | Function |  |
| `EnumSettingEntry` |  | Type | Jomini Settings - Enum Setting |
| `EnumSettingEntry.AccessSelf` | [unregistered] | Function |  |
| `EnumSettingEntry.AccessSetting` | [unregistered] | Function |  |
| `EnumSettingEntry.GetEnumString` | CString | Function |  |
| `EnumSettingEntry.GetGUIName` | CUTF8String | Function |  |
| `EnumSettingEntry.GetRecommendedText` | CString | Function |  |
| `EnumSettingEntry.GetTooltip` | CUTF8String | Function |  |
| `EnumSettingEntry.HasValueChanged` | bool | Function |  |
| `EnumSettingEntry.IsSelected` | bool | Function |  |
| `EnumSettingEntry.Self` | [unregistered] | Function |  |
| `Ethnicity` |  | Type | Portrait System - Ethnicity |
| `Ethnicity.AccessSelf` | [unregistered] | Function |  |
| `Ethnicity.GetName` | CString | Function |  |
| `Ethnicity.IsVisible` | bool | Function |  |
| `Ethnicity.MakeScope` | Scope | Promote |  |
| `Ethnicity.Self` | [unregistered] | Function |  |
| `ExecuteConsoleCommand( Arg0 )` | void | Global function | Execute a single console command with a given command. |
| `ExecuteConsoleCommands( Arg0 )` | void | Global function | Execute a sequence of console commands split by ';'. And stops the sequence execution if a console command fai… |
| `ExecuteConsoleCommandsForced( Arg0 )` | void | Global function | Execute a sequence of console commands split by ';'. Keeps the sequence execution even if a console command fa… |
| `Friend` |  | Type | Jomini Friend List - Friend |
| `Friend.AccessSelf` | [unregistered] | Function |  |
| `Friend.GetAvailabilityString` | CUTF8String | Function |  |
| `Friend.GetName` | CUTF8String | Function |  |
| `Friend.GetNameWithColor` | CUTF8String | Function |  |
| `Friend.GetUnreadMessageCount` | int32 | Function |  |
| `Friend.HasUnreadMessages` | bool | Function |  |
| `Friend.RemoveWithConfirmation` | void | Function |  |
| `Friend.Self` | [unregistered] | Function |  |
| `Friend.StartChat` | void | Function |  |
| `FriendListWindow` |  | Type | Jomini Friend List - Friendlist window |
| `FriendListWindow.AccessSelf` | [unregistered] | Function |  |
| `FriendListWindow.Hide` | void | Function |  |
| `FriendListWindow.Self` | [unregistered] | Function |  |
| `FriendListWindow.ShouldBeShown` | bool | Function |  |
| `FriendListWindow.Show` | void | Function |  |
| `FriendListWindow.ShowAddFriendWindow` | void | Function |  |
| `FriendRequest` |  | Type | Jomini Friend List - FriendRequest |
| `FriendRequest.Accept` | void | Function |  |
| `FriendRequest.AccessSelf` | [unregistered] | Function |  |
| `FriendRequest.GetName` | CUTF8String | Function |  |
| `FriendRequest.Reject` | void | Function |  |
| `FriendRequest.Self` | [unregistered] | Function |  |
| `Friends` |  | Type | Jomini Friend List - Friends manager |
| `Friends.AccessAvailabilityEntries` | [unregistered] | Function |  |
| `Friends.AccessFriendRequests` | [unregistered] | Function |  |
| `Friends.AccessFriends` | [unregistered] | Function |  |
| `Friends.AccessFriendSearchResults` | [unregistered] | Function |  |
| `Friends.AccessOutgoingFriendRequests` | [unregistered] | Function |  |
| `Friends.AccessSelf` | [unregistered] | Function |  |
| `Friends.CalcNumberOfOnlineFriends` | int32 | Function |  |
| `Friends.GetAvailabilityStringForLocalAccount` | CUTF8String | Function |  |
| `Friends.GetLocalProfileName` | CUTF8String | Function |  |
| `Friends.GetSearchString` | CUTF8String | Function |  |
| `Friends.GetSelectedAvailabilityEntryIndex` | CVector2i | Function |  |
| `Friends.HasIngoingFriendRequests` | bool | Function |  |
| `Friends.HasOutgoingFriendRequests` | bool | Function |  |
| `Friends.Search` | void | Function |  |
| `Friends.Self` | [unregistered] | Function |  |
| `Friends.SetSearchString` | void | Function |  |
| `Friends.SetSelectedAvailabilityEntryIndex` | void | Function |  |
| `Friends.UpdateFriends` | void | Function |  |
| `FriendSearchResult` |  | Type | Jomini Friends - Search Result |
| `FriendSearchResult.AccessSelf` | [unregistered] | Function |  |
| `FriendSearchResult.AddFriend` | void | Function |  |
| `FriendSearchResult.GetName` | CUTF8String | Function |  |
| `FriendSearchResult.Self` | [unregistered] | Function |  |
| `FrontEndCreditsView` |  | Type | Jomini Frontend Credits View |
| `FrontEndCreditsView.AccessSelf` | [unregistered] | Function |  |
| `FrontEndCreditsView.OnBack` | void | Function |  |
| `FrontEndCreditsView.Self` | [unregistered] | Function |  |
| `FrontEndMultiplayerView` |  | Type | Jomini Multiplayer - Frontendview |
| `FrontEndMultiplayerView.AccessSelf` | [unregistered] | Function |  |
| `FrontEndMultiplayerView.OnBack` | void | Function |  |
| `FrontEndMultiplayerView.OnHost` | void | Function |  |
| `FrontEndMultiplayerView.OnLoad` | void | Function |  |
| `FrontEndMultiplayerView.OnNewGame` | void | Function |  |
| `FrontEndMultiplayerView.Self` | [unregistered] | Function |  |
| `GeneCategory` |  | Type | Portrait System - Gene Category |
| `GeneCategory.AccessSelf` | [unregistered] | Function |  |
| `GeneCategory.GetName` | CString | Function |  |
| `GeneCategory.IsType( Arg0 )` | bool | Function |  |
| `GeneCategory.Self` | [unregistered] | Function |  |
| `GeneTemplate` |  | Type | Portrait System - Gene Template |
| `GeneTemplate.AccessSelf` | [unregistered] | Function |  |
| `GeneTemplate.GetName` | CString | Function |  |
| `GeneTemplate.Self` | [unregistered] | Function |  |
| `GetCanGoToFrontend` | bool | Global function | Multiplayer GUI - Can we go back to the frontend? |
| `GetCanGoToFrontendDesc` | CString | Global function | Multiplayer GUI - Message if we can't go back to the frontend |
| `GetColorDLC( Arg0 )` | CVector4f | Global function |  |
| `GetEnumIndex( Arg0 )` | CVector2i | Global function | Settings - Get the dropdown index of an enum setting. |
| `GetEthnicities` | [unregistered] | Global function | Portrait System - Get all Ethnicities |
| `GetGuiPositionFromPercentCoordinates( Arg0, Arg1, Arg2 )` | [unregistered] | Global function |  |
| `GetHostError` | CUTF8String | Global function | Multiplayer - Get host error |
| `GetInvParentScale( Arg0 )` | float | Global function |  |
| `GetIsChecked( Arg0 )` | bool | Global function | Settings - If the bool setting is checked |
| `GetPlayersCount` | int32 | Global function | Multiplayer - No of players of current session |
| `GetPortraitTextureFromDna( Arg0, Arg1 )` | [unregistered] | Global function |  |
| `GetRawTextTooltipTag( Arg0 )` | CString | Global function |  |
| `GetScriptedGui( Arg0 )` | ScriptedGui | Global promote | Jomini Scripted GUI - Get scripted GUI by name |
| `GetServerInfo` | ServerInformation | Global promote | Multiplayer - Server Info of current session |
| `GetStringSettingText( Arg0 )` | CString | Global function | Settings - Get the string setting text |
| `GoToFrontend` | void | Global function | Multiplayer GUI - Go back to the frontend |
| `Group` |  | Type | Jomini Achievement Window - Group |
| `Group.AccessSelf` | [unregistered] | Function |  |
| `Group.AccessVisibleAchievements` | [unregistered] | Function |  |
| `Group.CalcNumberOfUnlockedVisibleAchievements` | int32 | Function |  |
| `Group.GetName` | CUTF8String | Function |  |
| `Group.GetNumberOfVisibleAchievements` | int32 | Function |  |
| `Group.IsCollapsed` | bool | Function |  |
| `Group.Self` | [unregistered] | Function |  |
| `Group.ToggleCollapsed` | void | Function |  |
| `GUIAchievement` |  | Type | Jomini Achievement GUI |
| `GUIAchievement.AccessLockedTexture` | [unregistered] | Function |  |
| `GUIAchievement.AccessSelf` | [unregistered] | Function |  |
| `GUIAchievement.AccessTexture` | [unregistered] | Function |  |
| `GUIAchievement.GetAchievement` | Achievement | Promote |  |
| `GUIAchievement.GetAchievement` | [unregistered] | Function |  |
| `GUIAchievement.GetDescription` | CUTF8String | Function |  |
| `GUIAchievement.GetDLCKey` | CString | Function |  |
| `GUIAchievement.GetHappenedDescription` | CUTF8String | Function |  |
| `GUIAchievement.GetName` | CUTF8String | Function |  |
| `GUIAchievement.IsPossible` | bool | Function |  |
| `GUIAchievement.OnClick` | void | Function |  |
| `GUIAchievement.Remove` | void | Function |  |
| `GUIAchievement.Self` | [unregistered] | Function |  |
| `GuiContext` |  | Type | GUI Context |
| `GuiContext.AccessSelf` | [unregistered] | Function |  |
| `GuiContext.GetScreenResolution` | CVector2f | Function |  |
| `GuiContext.Self` | [unregistered] | Function |  |
| `GuiCurrentDrag` |  | Type | For use in drag_widget, inside of draggable_behavior. Contains data regarding current ongoing drag, such as mo… |
| `GuiCurrentDrag.AccessSelf` | [unregistered] | Function |  |
| `GuiCurrentDrag.CurrentPosition` | CVector2f | Function |  |
| `GuiCurrentDrag.Self` | [unregistered] | Function |  |
| `GuiCurrentDrag.StartPosition` | CVector2f | Function |  |
| `GuiScope` | TopScope | Global promote | Jomini Scripted GUI - Get GUI top jomini scope |
| `GuiTableRow` |  | Type | Generic Table - Row |
| `GuiTableRow.AccessSelf` | [unregistered] | Function |  |
| `GuiTableRow.GetIndex` | int32 | Function |  |
| `GuiTableRow.Self` | [unregistered] | Function |  |
| `HasContext( Arg0 )` | bool | Global function | Returns true if the widget has a data context of the type of the given name |
| `HasHostError` | bool | Global function | Multiplayer - Does the host have an error |
| `HasNonConstContext( Arg0 )` | bool | Global function | Returns true if the widget has a non-const data context of the type of the given name |
| `ImageLookupNode` |  | Type | Image Look up node window |
| `ImageLookupNode.AccessSelf` | [unregistered] | Function |  |
| `ImageLookupNode.Self` | [unregistered] | Function |  |
| `InputActionBinding` |  | Type | Input Context Manager - Input Action |
| `InputActionBinding.AccessSelf` | [unregistered] | Function |  |
| `InputActionBinding.Self` | [unregistered] | Function |  |
| `IsPreparationLobby` | bool | Global function |  |
| `JominiAccessPlayerJoinRequests` | [unregistered] | Global function |  |
| `JominiAreAchievementsAvailable` | bool | Global function | Jomini Achievements - Are achievements available? |
| `JominiGetAchievementsNotAvailableString` | CString | Global function | Jomini Achievements - Get text why achievements are not available |
| `JominiGetMultiplayerAccessibleString` | CString | Global function |  |
| `JominiGUISetting` |  | Type | Jomini Setting |
| `JominiGUISetting.AccessSelf` | [unregistered] | Function |  |
| `JominiGUISetting.GetBlockedReason` | CString | Function |  |
| `JominiGUISetting.GetDesc` | CString | Function |  |
| `JominiGUISetting.GetRelationDisabledReason` | CString | Function |  |
| `JominiGUISetting.HasChanged` | bool | Function |  |
| `JominiGUISetting.HasChangedSinceStartup` | bool | Function |  |
| `JominiGUISetting.IsBlocked` | bool | Function |  |
| `JominiGUISetting.IsModificationEnabled` | bool | Function |  |
| `JominiGUISetting.Self` | [unregistered] | Function |  |
| `JominiGUISetting.ShouldShow` | bool | Function |  |
| `JominiHasPlayerJoinRequests` | bool | Global function |  |
| `JominiIsHostOrLocal` | bool | Global function |  |
| `JominiIsMultiplayerAccessible` | bool | Global function |  |
| `JominiMultiplayerIsCrossplayEnabled` | bool | Global function |  |
| `JominiMultiplayerIsCrossplayFilterAvailable` | bool | Global function |  |
| `JominiNotification` |  | Type | Jomini Notification Item |
| `JominiNotification.AccessSelf` | [unregistered] | Function |  |
| `JominiNotification.GetAcceptButtonText` | CString | Function |  |
| `JominiNotification.GetDate` | [unregistered] | Function |  |
| `JominiNotification.GetDate` | Date | Promote |  |
| `JominiNotification.GetDateOrTimestamp` | CString | Function |  |
| `JominiNotification.GetDeclineButtonText` | CString | Function |  |
| `JominiNotification.GetText` | CString | Function |  |
| `JominiNotification.GetTimestamp` | CString | Function |  |
| `JominiNotification.GetTitle` | CString | Function |  |
| `JominiNotification.HasDate` | bool | Function |  |
| `JominiNotification.HasOnAccept` | bool | Function |  |
| `JominiNotification.HasOnDecline` | bool | Function |  |
| `JominiNotification.IsPassword` | bool | Function |  |
| `JominiNotification.OnAccept` | void | Function |  |
| `JominiNotification.OnDecline` | void | Function |  |
| `JominiNotification.OnNotificationClick` | void | Function |  |
| `JominiNotification.Self` | [unregistered] | Function |  |
| `JominiNotification.SetupDataContexts( Arg0 )` | NotificationDummyContext | Promote |  |
| `JominiNotificationOverlay` |  | Type |  |
| `JominiNotificationOverlay.AccessSelf` | [unregistered] | Function |  |
| `JominiNotificationOverlay.GetAlertCount` | int32 | Function |  |
| `JominiNotificationOverlay.GetAlerts` | [unregistered] | Function |  |
| `JominiNotificationOverlay.GetNotificationCategoryCount( Arg0 )` | int32 | Function |  |
| `JominiNotificationOverlay.GetNotificationCount` | int32 | Function |  |
| `JominiNotificationOverlay.GetNotifications` | [unregistered] | Function |  |
| `JominiNotificationOverlay.HideSidebar` | void | Function |  |
| `JominiNotificationOverlay.Self` | [unregistered] | Function |  |
| `JominiNotificationOverlay.ShowSidebar` | void | Function |  |
| `JominiNotificationOverlay.ToggleSidebar` | void | Function |  |
| `JominiPlayer` | Playable | Global promote | Jomini Script Systen - Get generic current playable object |
| `JominiPlayer` | [unregistered] | Global function | Jomini Script Systen - Get generic current playable object |
| `JominiServer` |  | Type | Jomini Multiplayer - Server |
| `JominiServer.AccessSelf` | [unregistered] | Function |  |
| `JominiServer.GetHasPassword` | bool | Function |  |
| `JominiServer.GetMaxPlayers` | int32 | Function |  |
| `JominiServer.GetName` | CString | Function |  |
| `JominiServer.GetNumPlayers` | int32 | Function |  |
| `JominiServer.GetStatus` | CString | Function |  |
| `JominiServer.GetVersion` | CString | Function |  |
| `JominiServer.IsSameVersion` | bool | Function |  |
| `JominiServer.Join` | void | Function |  |
| `JominiServer.Self` | [unregistered] | Function |  |
| `JominiServerBrowserGui` |  | Type | Jomini Multiplayer - Server Browser |
| `JominiServerBrowserGui.AccessSelf` | [unregistered] | Function |  |
| `JominiServerBrowserGui.AccessServers` | [unregistered] | Function |  |
| `JominiServerBrowserGui.GetServers` | [unregistered] | Function |  |
| `JominiServerBrowserGui.IsCrossPlatformFilterSet` | bool | Function |  |
| `JominiServerBrowserGui.IsEmptyFilterSet` | bool | Function |  |
| `JominiServerBrowserGui.IsFullFilterSet` | bool | Function |  |
| `JominiServerBrowserGui.IsHasPasswordFilterSet` | bool | Function |  |
| `JominiServerBrowserGui.IsRefreshing` | bool | Function |  |
| `JominiServerBrowserGui.IsSameModsFilterSet` | bool | Function |  |
| `JominiServerBrowserGui.IsSameVersionFilterSet` | bool | Function |  |
| `JominiServerBrowserGui.IsSortAscending` | bool | Function |  |
| `JominiServerBrowserGui.IsSortedBy( Arg0 )` | bool | Function |  |
| `JominiServerBrowserGui.JoinServerWithId` | void | Function |  |
| `JominiServerBrowserGui.JoinServerWithTextWidget( Arg0 )` | void | Function |  |
| `JominiServerBrowserGui.OnFilterTextEdit` | void | Function |  |
| `JominiServerBrowserGui.RefreshServers` | void | Function |  |
| `JominiServerBrowserGui.Self` | [unregistered] | Function |  |
| `JominiServerBrowserGui.SetFilterText` | void | Function |  |
| `JominiServerBrowserGui.SetServerId` | void | Function |  |
| `JominiServerBrowserGui.Sort( Arg0 )` | void | Function |  |
| `JominiServerBrowserGui.ToggleCrossPlatformFilter` | void | Function |  |
| `JominiServerBrowserGui.ToggleEmptyFilter` | void | Function |  |
| `JominiServerBrowserGui.ToggleFullFilter` | void | Function |  |
| `JominiServerBrowserGui.ToggleHasPasswordFilter` | void | Function |  |
| `JominiServerBrowserGui.ToggleSameModsFilter` | void | Function |  |
| `JominiServerBrowserGui.ToggleSameVersionFilter` | void | Function |  |
| `JominiSettingsWindow` |  | Type | Jomini Settings Window |
| `JominiSettingsWindow.AccessActivePage` | SettingsPage | Promote |  |
| `JominiSettingsWindow.AccessActivePage` | [unregistered] | Function |  |
| `JominiSettingsWindow.AccessPages` | [unregistered] | Function |  |
| `JominiSettingsWindow.AccessSelf` | [unregistered] | Function |  |
| `JominiSettingsWindow.AllowApply` | bool | Function |  |
| `JominiSettingsWindow.AppliedOnApply` | bool | Function |  |
| `JominiSettingsWindow.AppliedOnRestart` | bool | Function |  |
| `JominiSettingsWindow.Close` | void | Function |  |
| `JominiSettingsWindow.GetInputActionName( Arg0 )` | CString | Function |  |
| `JominiSettingsWindow.GetInputBindingText` | CString | Function |  |
| `JominiSettingsWindow.GetKeyCodeName( Arg0, Arg1 )` | CString | Function |  |
| `JominiSettingsWindow.GetUserBindingFileName` | CString | Function |  |
| `JominiSettingsWindow.GetWarningText` | CString | Function |  |
| `JominiSettingsWindow.HasChanged` | bool | Function |  |
| `JominiSettingsWindow.HasUserBindingFileName` | bool | Function |  |
| `JominiSettingsWindow.Hide` | void | Function |  |
| `JominiSettingsWindow.IsConfirmButtonEnabled` | bool | Function |  |
| `JominiSettingsWindow.IsCurrentActivePageId( Arg0 )` | bool | Function |  |
| `JominiSettingsWindow.IsInputPopUpEnabled` | bool | Function |  |
| `JominiSettingsWindow.OnApply` | void | Function |  |
| `JominiSettingsWindow.OnClickCancel` | void | Function |  |
| `JominiSettingsWindow.OnClickClear` | void | Function |  |
| `JominiSettingsWindow.OnClickConfirm` | void | Function |  |
| `JominiSettingsWindow.OnClickLoadDefaultProfile` | void | Function |  |
| `JominiSettingsWindow.OnClickLoadProfile` | void | Function |  |
| `JominiSettingsWindow.OnClickSaveProfile` | void | Function |  |
| `JominiSettingsWindow.OnEditProfileFileName` | void | Function |  |
| `JominiSettingsWindow.RequireRestart` | bool | Function |  |
| `JominiSettingsWindow.Restore` | void | Function |  |
| `JominiSettingsWindow.Save` | void | Function |  |
| `JominiSettingsWindow.SaveAndClose` | void | Function |  |
| `JominiSettingsWindow.Self` | [unregistered] | Function |  |
| `JominiSettingsWindow.ShouldShowWarningText` | bool | Function |  |
| `JominiSettingsWindow.Show` | void | Function |  |
| `JominiSettingsWindow.ShowInputPopUp( Arg0, Arg1, Arg2 )` | void | Function |  |
| `LegalDocsViewer` |  | Type | Legal Documents Viewer |
| `LegalDocsViewer.AccessSelf` | [unregistered] | Function |  |
| `LegalDocsViewer.Agree` | void | Function |  |
| `LegalDocsViewer.GetDocumentText` | CUTF8String | Function |  |
| `LegalDocsViewer.Hide` | void | Function |  |
| `LegalDocsViewer.Self` | [unregistered] | Function |  |
| `LegalDocsViewer.Show` | void | Function |  |
| `LobbyPlayer` |  | Type | Jomini Multiplayer - Lobby Player |
| `LobbyPlayer.AccessSelf` | [unregistered] | Function |  |
| `LobbyPlayer.CanRemovePlayer` | bool | Function |  |
| `LobbyPlayer.GetID` | int32 | Function |  |
| `LobbyPlayer.GetLoadingString` | CString | Function |  |
| `LobbyPlayer.GetLobbyView` | LobbyView | Promote |  |
| `LobbyPlayer.GetName` | CString | Function |  |
| `LobbyPlayer.GetObservedPlayable` | Playable | Promote |  |
| `LobbyPlayer.GetPlayable` | Playable | Promote |  |
| `LobbyPlayer.GetTransferProgress` | float | Function |  |
| `LobbyPlayer.HasObservedPlayable` | bool | Function |  |
| `LobbyPlayer.HasPlayable` | bool | Function |  |
| `LobbyPlayer.IsDoneReplayingCommands` | bool | Function |  |
| `LobbyPlayer.IsHost` | bool | Function |  |
| `LobbyPlayer.IsHotjoining` | bool | Function |  |
| `LobbyPlayer.IsLocalPlayer` | bool | Function |  |
| `LobbyPlayer.IsObserver` | bool | Function |  |
| `LobbyPlayer.IsReady` | bool | Function |  |
| `LobbyPlayer.RemovePlayer` | void | Function |  |
| `LobbyPlayer.Self` | [unregistered] | Function |  |
| `LobbyPlayer.ShowReadyStatus` | bool | Function |  |
| `LobbyView` |  | Type | Multiplayer GUI - Lobby View |
| `LobbyView.AccessLocalPlayer` | LobbyPlayer | Promote |  |
| `LobbyView.AccessSelectedPlayable` | Playable | Promote |  |
| `LobbyView.AccessSelectedPlayable` | [unregistered] | Function |  |
| `LobbyView.AccessSelf` | [unregistered] | Function |  |
| `LobbyView.CanControlSelected` | bool | Function |  |
| `LobbyView.CanGoBack` | bool | Function |  |
| `LobbyView.CanReady` | bool | Function |  |
| `LobbyView.CanStart` | bool | Function |  |
| `LobbyView.ClearPreSelected` | void | Function |  |
| `LobbyView.ClearSelection` | void | Function |  |
| `LobbyView.ControlSelected` | void | Function |  |
| `LobbyView.CopyServerID` | void | Function |  |
| `LobbyView.GetControlToolTip` | CString | Function |  |
| `LobbyView.GetEnabledDLCs` | [unregistered] | Function |  |
| `LobbyView.GetEnabledMods` | [unregistered] | Function |  |
| `LobbyView.GetGoBackToolTip` | CString | Function |  |
| `LobbyView.GetOpenMultiplayerToolTip` | CString | Function |  |
| `LobbyView.GetOtherPlayersCount` | int32 | Function |  |
| `LobbyView.GetReadyToolTip` | CString | Function |  |
| `LobbyView.GetSelectedPlayable` | Playable | Promote |  |
| `LobbyView.GetSelectText` | CString | Function |  |
| `LobbyView.GetServerInfo` | ServerInformation | Promote |  |
| `LobbyView.GoBack` | void | Function |  |
| `LobbyView.HasEnabledDLCs` | bool | Function |  |
| `LobbyView.HasEnabledMods` | bool | Function |  |
| `LobbyView.HasGameConfigurationWindow` | bool | Function |  |
| `LobbyView.HasSelectedPlayable` | bool | Function |  |
| `LobbyView.HasServerInfo` | bool | Function |  |
| `LobbyView.IsAutoAccept` | bool | Function |  |
| `LobbyView.IsInvite` | bool | Function |  |
| `LobbyView.IsObserver` | bool | Function |  |
| `LobbyView.IsPublicCrossplay` | bool | Function |  |
| `LobbyView.IsPublicNoCrossplay` | bool | Function |  |
| `LobbyView.IsReady` | bool | Function |  |
| `LobbyView.OnNameEdit` | void | Function |  |
| `LobbyView.OnPasswordEdit` | void | Function |  |
| `LobbyView.OpenGameConfigurationWindow` | void | Function |  |
| `LobbyView.OpenMultiplayer` | void | Function |  |
| `LobbyView.Players` | [unregistered] | Function |  |
| `LobbyView.Ready` | void | Function |  |
| `LobbyView.Self` | [unregistered] | Function |  |
| `LobbyView.SetInvite` | void | Function |  |
| `LobbyView.SetName` | void | Function |  |
| `LobbyView.SetPassword` | void | Function |  |
| `LobbyView.SetPublicCrossplay` | void | Function |  |
| `LobbyView.SetPublicNoCrossplay` | void | Function |  |
| `LobbyView.SetShowPlayerList( Arg0 )` | void | Function |  |
| `LobbyView.ShouldShowEnabledDLCs` | bool | Function |  |
| `LobbyView.ShouldShowEnabledMods` | bool | Function |  |
| `LobbyView.ShowHosting` | bool | Function |  |
| `LobbyView.ShowModifyServer` | bool | Function |  |
| `LobbyView.ShowPlayerList` | bool | Function |  |
| `LobbyView.Start` | void | Function |  |
| `LobbyView.StartToolTip` | CString | Function |  |
| `LobbyView.ToggleAutoAccept` | void | Function |  |
| `LobbyView.ToggleObserve` | void | Function |  |
| `LobbyView.ToggleShowEnabledDLCs` | void | Function |  |
| `LobbyView.ToggleShowEnabledMods` | void | Function |  |
| `LobbyView.ToggleVisibility` | void | Function |  |
| `LobbyView.UnReady` | void | Function |  |
| `LocalMod` |  | Type | Mods Gui - Local Mod |
| `LocalMod.AccessSelf` | [unregistered] | Function |  |
| `LocalMod.Description` | CString | Function |  |
| `LocalMod.DisplayImage` | [unregistered] | Function |  |
| `LocalMod.DisplayName` | CString | Function |  |
| `LocalMod.GetFilesAffectingChecksum` | CString | Function |  |
| `LocalMod.GetIssues` | CString | Function |  |
| `LocalMod.GetNonChecksumIssues` | CString | Function |  |
| `LocalMod.HasIssues` | bool | Function |  |
| `LocalMod.HasNonChecksumIssues` | bool | Function |  |
| `LocalMod.Id` | int32 | Function |  |
| `LocalMod.IsAffectingChecksum` | bool | Function |  |
| `LocalMod.IsChecksumCompatibilityCalculated` | bool | Function |  |
| `LocalMod.IsExternal` | bool | Function |  |
| `LocalMod.IsPdxMod` | bool | Function |  |
| `LocalMod.MatchesSearch( Arg0 )` | bool | Function |  |
| `LocalMod.Name` | CString | Function |  |
| `LocalMod.OpenInFolder` | void | Function |  |
| `LocalMod.Path` | CString | Function |  |
| `LocalMod.Self` | [unregistered] | Function |  |
| `LocalMod.SupportedGameVersion` | CString | Function |  |
| `LocalMod.Version` | CString | Function |  |
| `LoginView` |  | Type | Login View |
| `LoginView.AccessSelf` | [unregistered] | Function |  |
| `LoginView.GetEmail` | CUTF8String | Function |  |
| `LoginView.GetErrorMessage` | CUTF8String | Function |  |
| `LoginView.GetLoginString` | CUTF8String | Function |  |
| `LoginView.GetPassword` | CUTF8String | Function |  |
| `LoginView.Hide` | void | Function |  |
| `LoginView.IsChangingUserName` | bool | Function |  |
| `LoginView.IsConnectingAccount` | bool | Function |  |
| `LoginView.IsEmailSent` | bool | Function |  |
| `LoginView.IsEmptyUserNamePrompt` | bool | Function |  |
| `LoginView.IsLoggedIn` | bool | Function |  |
| `LoginView.IsLogin` | bool | Function |  |
| `LoginView.IsRestoringPassword` | bool | Function |  |
| `LoginView.IsSuccessfulSetName` | bool | Function |  |
| `LoginView.IsValidLoginInfo` | bool | Function |  |
| `LoginView.Login` | void | Function |  |
| `LoginView.Logout` | void | Function |  |
| `LoginView.OnUserNameChange` | void | Function |  |
| `LoginView.ResetPassword` | void | Function |  |
| `LoginView.Self` | [unregistered] | Function |  |
| `LoginView.SetEmail` | void | Function |  |
| `LoginView.SetPassword` | void | Function |  |
| `LoginView.SetState( Arg0 )` | void | Function |  |
| `LoginView.SetUserName` | void | Function |  |
| `LoginView.Show` | void | Function |  |
| `LoginView.ToggleConnected` | void | Function |  |
| `MarketingContainer` |  | Type |  |
| `MarketingContainer.AccessSelf` | [unregistered] | Function |  |
| `MarketingContainer.AccessSlotByName( Arg0 )` | [unregistered] | Function |  |
| `MarketingContainer.AccessSlotByName( Arg0 )` | MarketingSlot | Promote |  |
| `MarketingContainer.GetSlotByName( Arg0 )` | [unregistered] | Function |  |
| `MarketingContainer.GetSlotByName( Arg0 )` | MarketingSlot | Promote |  |
| `MarketingContainer.RequestData` | void | Function |  |
| `MarketingContainer.Self` | [unregistered] | Function |  |
| `MarketingSlot` |  | Type |  |
| `MarketingSlot.AccessSelf` | [unregistered] | Function |  |
| `MarketingSlot.ClearStartAnimating` | void | Function |  |
| `MarketingSlot.ClearStopAnimating` | void | Function |  |
| `MarketingSlot.ClickActiveCard` | void | Function |  |
| `MarketingSlot.CycleActiveContentCard` | void | Function |  |
| `MarketingSlot.GetCarouselDelay` | float | Function |  |
| `MarketingSlot.GetNextTexture` | [unregistered] | Function |  |
| `MarketingSlot.GetTexture` | [unregistered] | Function |  |
| `MarketingSlot.HasContent` | bool | Function |  |
| `MarketingSlot.Self` | [unregistered] | Function |  |
| `MarketingSlot.StartAnimating` | bool | Function |  |
| `MarketingSlot.StopAnimating` | bool | Function |  |
| `MeshImporterMaterialEntry` |  | Type |  |
| `MeshImporterMaterialEntry.AccessSelf` | [unregistered] | Function |  |
| `MeshImporterMaterialEntry.GetDetailsDescription` | CString | Function |  |
| `MeshImporterMaterialEntry.GetMaterialIndex` | int32 | Function |  |
| `MeshImporterMaterialEntry.GetName` | CString | Function |  |
| `MeshImporterMaterialEntry.GetNumberGeometriesUsingMaterial` | int32 | Function |  |
| `MeshImporterMaterialEntry.InjectPropertyList( Arg0, Arg1, Arg2 )` | void | Function |  |
| `MeshImporterMaterialEntry.IsCollision` | bool | Function |  |
| `MeshImporterMaterialEntry.Self` | [unregistered] | Function |  |
| `MeshImporterMaterialEntry.ToggleCollision` | void | Function |  |
| `MeshImporterMaterials` |  | Type |  |
| `MeshImporterMaterials.AccessMaterialEntries` | [unregistered] | Function |  |
| `MeshImporterMaterials.AccessSelf` | [unregistered] | Function |  |
| `MeshImporterMaterials.GetMaterialEntries` | [unregistered] | Function |  |
| `MeshImporterMaterials.Self` | [unregistered] | Function |  |
| `ModsGui` |  | Type | Mods Gui Data Context |
| `ModsGui.AccessSelf` | [unregistered] | Function |  |
| `ModsGui.ActivePlayset` | ModsPlayset | Promote |  |
| `ModsGui.ApplyText` | CString | Function |  |
| `ModsGui.ApplyTooltip` | CString | Function |  |
| `ModsGui.AreAllDLCsToggled` | bool | Function |  |
| `ModsGui.AreAllModsToggled` | bool | Function |  |
| `ModsGui.BrowseModsTooltip` | CString | Function |  |
| `ModsGui.CanApply` | bool | Function |  |
| `ModsGui.CanBrowseMods` | bool | Function |  |
| `ModsGui.CanCreatePlayset( Arg0 )` | bool | Function |  |
| `ModsGui.ClearActivePlayset` | void | Function |  |
| `ModsGui.ClearSearchString` | void | Function |  |
| `ModsGui.ClearSelectedPlayset` | void | Function |  |
| `ModsGui.Close` | void | Function |  |
| `ModsGui.ConfirmDeletePlayset( Arg0 )` | void | Function |  |
| `ModsGui.CreateMod( Arg0 )` | void | Function | Create a new empty mod in the user mod directory. |
| `ModsGui.CreatePlayset( Arg0 )` | void | Function |  |
| `ModsGui.CreatePlaysetTooltip( Arg0 )` | CString | Function |  |
| `ModsGui.DeletePlayset( Arg0 )` | bool | Function |  |
| `ModsGui.DownloadAndApplyChanges` | void | Function | Syncs pending data and saves the current changes to disk. |
| `ModsGui.ErrorString` | CString | Function |  |
| `ModsGui.FilteredLocalMods` | [unregistered] | Function |  |
| `ModsGui.FilteredNonDownloadedMods` | [unregistered] | Function |  |
| `ModsGui.GetActivePlaysetDropdownIndex` | CVector2i | Function | Gets array index for the active playset. |
| `ModsGui.GetActivePlaysetName` | CString | Function | Gets array index for the active playset. |
| `ModsGui.GetPlayset( Arg0 )` | ModsPlayset | Promote |  |
| `ModsGui.HasError` | bool | Function |  |
| `ModsGui.HasExternalInterface` | bool | Function | Returns true if the mods backend has an external interface (e.g. steam overlay). |
| `ModsGui.HasGeneralError` | bool | Function |  |
| `ModsGui.HasModError` | bool | Function |  |
| `ModsGui.HasPendingChanges` | bool | Function |  |
| `ModsGui.HasPlaysetError` | bool | Function |  |
| `ModsGui.InteractionsAllowed` | bool | Function |  |
| `ModsGui.IsOpen` | bool | Function |  |
| `ModsGui.IsPlaysetActive( Arg0 )` | bool | Function |  |
| `ModsGui.IsPlaysetApplied( Arg0 )` | bool | Function |  |
| `ModsGui.IsRefreshModsAffectingChecksumAvailable` | bool | Function |  |
| `ModsGui.IsSearchingMods` | bool | Function |  |
| `ModsGui.IsValid` | bool | Function |  |
| `ModsGui.LocalizedError` | CString | Function |  |
| `ModsGui.LocalModCount` | int32 | Function |  |
| `ModsGui.LocalMods` | [unregistered] | Function |  |
| `ModsGui.ModifiablePlaysets` | [unregistered] | Function |  |
| `ModsGui.ModWithError` | int32 | Function |  |
| `ModsGui.NonDownloadedMods` | [unregistered] | Function |  |
| `ModsGui.OpenExternalInterface` | void | Function | Open the external interface for mods (e.g. steam overlay). |
| `ModsGui.Playsets` | [unregistered] | Function |  |
| `ModsGui.PlaysetWithError` | int32 | Function |  |
| `ModsGui.Progress` | float | Function |  |
| `ModsGui.RefreshModsAffectingChecksum` | void | Function |  |
| `ModsGui.ResyncLocalMods` | void | Function | Full Resync of mods present on disk. All local changes made since the last call to ApplyChanges are lost. |
| `ModsGui.SavePlayset` | void | Function | Saves the current changed data in the playsets to disk. |
| `ModsGui.SearchString` | CUTF8String | Function |  |
| `ModsGui.SelectedPlaysetActiveDlcCount` | int32 | Function |  |
| `ModsGui.SelectedPlaysetDlcCount` | int32 | Function |  |
| `ModsGui.SelectedPlaysetHasVersionIssues` | bool | Function | Returns true if the selected playset has a mod inside with a version that doesn't match the current version. |
| `ModsGui.SelectedPlaysetId` | int32 | Function |  |
| `ModsGui.SelectedPlaysetModCount` | int32 | Function |  |
| `ModsGui.Self` | [unregistered] | Function |  |
| `ModsGui.SetActivePlaysetId( Arg0 )` | void | Function |  |
| `ModsGui.SetSearchString` | void | Function |  |
| `ModsGui.SetSelectedPlaysetId( Arg0 )` | void | Function |  |
| `ModsGui.StartSearchingForMods` | void | Function |  |
| `ModsGui.StartSearchingModsWithText` | void | Function |  |
| `ModsGui.ToggleAllDLCs` | void | Function |  |
| `ModsGui.ToggleAllMods` | void | Function |  |
| `ModsGui.UpdateModOnBackend( Arg0 )` | bool | Function |  |
| `ModsGui.UploadModToBackend( Arg0 )` | bool | Function |  |
| `MPConfig` |  | Type | Jomini Multiplayer - Configuration |
| `MPConfig.AccessSelf` | [unregistered] | Function |  |
| `MPConfig.GetOfflinePlayerToolTip` | CString | Function |  |
| `MPConfig.GetPrivateToolTip` | CString | Function |  |
| `MPConfig.GetPublicToolTip` | CString | Function |  |
| `MPConfig.IsCrossplayChecked` | bool | Function |  |
| `MPConfig.IsHotjoinAutoAccept` | bool | Function |  |
| `MPConfig.IsOffline` | bool | Function |  |
| `MPConfig.IsPrivate` | bool | Function |  |
| `MPConfig.IsPublic` | bool | Function |  |
| `MPConfig.Self` | [unregistered] | Function |  |
| `MPConfig.SetOffline` | void | Function |  |
| `MPConfig.SetPrivate` | void | Function |  |
| `MPConfig.SetPublic` | void | Function |  |
| `MPConfig.ToggleCrossplay` | void | Function |  |
| `MPConfig.ToggleHotjoinAutoAccept` | void | Function |  |
| `MultiplayerSetupWindow` |  | Type | Jomini Multiplayer - Setup window |
| `MultiplayerSetupWindow.AccessMPConfig` | MPConfig | Promote |  |
| `MultiplayerSetupWindow.AccessSelf` | [unregistered] | Function |  |
| `MultiplayerSetupWindow.Cancel` | void | Function |  |
| `MultiplayerSetupWindow.CanHost` | bool | Function |  |
| `MultiplayerSetupWindow.GetDefaultServerName` | CUTF8String | Function |  |
| `MultiplayerSetupWindow.GetDefaultServerPassword` | CUTF8String | Function |  |
| `MultiplayerSetupWindow.GetMPConfig` | MPConfig | Promote |  |
| `MultiplayerSetupWindow.Host` | void | Function |  |
| `MultiplayerSetupWindow.HostToolTip` | CString | Function |  |
| `MultiplayerSetupWindow.Self` | [unregistered] | Function |  |
| `MultiplayerSetupWindow.ShouldShowMPConfig` | bool | Function |  |
| `MusicPlayer` |  | Type |  |
| `MusicPlayer.AccessCategories` | [unregistered] | Function |  |
| `MusicPlayer.AccessSelf` | [unregistered] | Function |  |
| `MusicPlayer.AccessTracksFromSelectedCategory` | [unregistered] | Function |  |
| `MusicPlayer.GetCategoryEnabledText( Arg0 )` | CString | Function |  |
| `MusicPlayer.GetCategoryName( Arg0 )` | CString | Function |  |
| `MusicPlayer.GetCategoryNameLocKey( Arg0 )` | CString | Function |  |
| `MusicPlayer.GetMusicDensity` | float | Function |  |
| `MusicPlayer.GetMusicDensityString` | CString | Function |  |
| `MusicPlayer.GetPlaybackStatusFrameIndex` | int32 | Function |  |
| `MusicPlayer.GetPlayingTrackElapsedTime` | CString | Function |  |
| `MusicPlayer.GetPlayingTrackName` | CString | Function |  |
| `MusicPlayer.GetSelectedCategory` | [unregistered] | Function |  |
| `MusicPlayer.GetTrackEnabledText( Arg0 )` | CString | Function |  |
| `MusicPlayer.GetTrackName( Arg0 )` | CString | Function |  |
| `MusicPlayer.GetTrackTextColor( Arg0 )` | CVector4f | Function |  |
| `MusicPlayer.GetVolume` | float | Function |  |
| `MusicPlayer.GetVolumeString` | CString | Function |  |
| `MusicPlayer.HasSelectedCategory` | bool | Function |  |
| `MusicPlayer.IsPlaying` | bool | Function |  |
| `MusicPlayer.IsSelectedCategory( Arg0 )` | bool | Function |  |
| `MusicPlayer.OnNextTrack` | void | Function |  |
| `MusicPlayer.OnSelectAndPlayTrack( Arg0 )` | void | Function |  |
| `MusicPlayer.OnSelectCategory( Arg0 )` | void | Function |  |
| `MusicPlayer.OnSelectTrack( Arg0 )` | void | Function |  |
| `MusicPlayer.OnToggleCategoryEnabled( Arg0 )` | void | Function |  |
| `MusicPlayer.OnTogglePlayPause` | void | Function |  |
| `MusicPlayer.OnToggleTrackEnabled( Arg0 )` | void | Function |  |
| `MusicPlayer.Self` | [unregistered] | Function |  |
| `MusicPlayer.SetMusicDensity` | void | Function |  |
| `MusicPlayer.SetVolume` | void | Function |  |
| `MusicPlayerCategory` |  | Type |  |
| `MusicPlayerCategory.AccessSelf` | [unregistered] | Function |  |
| `MusicPlayerCategory.GetName` | CString | Function |  |
| `MusicPlayerCategory.GetNameLocKey` | CString | Function |  |
| `MusicPlayerCategory.GetTexture` | [unregistered] | Function |  |
| `MusicPlayerCategory.GetTracks` | [unregistered] | Function |  |
| `MusicPlayerCategory.HasTexture` | bool | Function |  |
| `MusicPlayerCategory.IsEnabled` | bool | Function |  |
| `MusicPlayerCategory.IsPlaying` | bool | Function |  |
| `MusicPlayerCategory.Self` | [unregistered] | Function |  |
| `MusicTrack` |  | Type |  |
| `MusicTrack.AccessSelf` | [unregistered] | Function |  |
| `MusicTrack.GetCategory` | [unregistered] | Function |  |
| `MusicTrack.GetName` | CString | Function |  |
| `MusicTrack.IsEnabled` | bool | Function |  |
| `MusicTrack.IsPlaying` | bool | Function |  |
| `MusicTrack.Self` | [unregistered] | Function |  |
| `NonDownloadedMod` |  | Type | Mods Gui - Non-Downloaded Mod |
| `NonDownloadedMod.AccessSelf` | [unregistered] | Function |  |
| `NonDownloadedMod.Description` | CString | Function |  |
| `NonDownloadedMod.DisplayImage` | [unregistered] | Function |  |
| `NonDownloadedMod.DisplayName` | CString | Function |  |
| `NonDownloadedMod.Id` | int32 | Function |  |
| `NonDownloadedMod.Name` | CString | Function |  |
| `NonDownloadedMod.Self` | [unregistered] | Function |  |
| `NonDownloadedMod.Version` | CString | Function |  |
| `NOP` | void | Global function | No Operation (dummy callback function) |
| `NotificationDummyContext` |  | Type | Jomini Notification Dummy Context |
| `NotificationDummyContext.AccessSelf` | [unregistered] | Function |  |
| `NotificationDummyContext.Self` | [unregistered] | Function |  |
| `OnCreateAccount` | void | Global function | Start the account creation flow |
| `OosData` |  | Type | Jomini Multiplayer - Out-of-sync data |
| `OosData.AccessSelf` | [unregistered] | Function |  |
| `OosData.GetFolder` | CString | Function |  |
| `OosData.GetPlayable` | Playable | Promote |  |
| `OosData.GetPlayerId` | int32 | Function |  |
| `OosData.GetPlayerName` | CString | Function |  |
| `OosData.GetType` | CString | Function |  |
| `OosData.IsLocalPlayer` | bool | Function |  |
| `OosData.IsObserver` | bool | Function |  |
| `OosData.OpenFileDirectory` | void | Function |  |
| `OosData.Self` | [unregistered] | Function |  |
| `OosWindow` |  | Type | Jomini Multiplayer - Out-of-sync window |
| `OosWindow.AccessSelf` | [unregistered] | Function |  |
| `OosWindow.Close` | void | Function |  |
| `OosWindow.Continue` | void | Function |  |
| `OosWindow.GetOosData` | [unregistered] | Function |  |
| `OosWindow.GoToMenu` | void | Function |  |
| `OosWindow.IsLocalOos` | bool | Function |  |
| `OosWindow.IsMultiplayerHost` | bool | Function |  |
| `OosWindow.Resync` | void | Function |  |
| `OosWindow.Self` | [unregistered] | Function |  |
| `OutgoingFriendRequest` |  | Type | Jomini Friend List - Outgoing friend request |
| `OutgoingFriendRequest.AccessSelf` | [unregistered] | Function |  |
| `OutgoingFriendRequest.GetName` | CUTF8String | Function |  |
| `OutgoingFriendRequest.Remove` | void | Function |  |
| `OutgoingFriendRequest.Self` | [unregistered] | Function |  |
| `PdxAccount` |  | Type |  |
| `PdxAccount.AccessSelf` | [unregistered] | Function |  |
| `PdxAccount.GetInventory` | AccountInventory | Promote |  |
| `PdxAccount.GetLastLinkError` | [unregistered] | Function |  |
| `PdxAccount.GetLastLinkErrorLocalized` | CString | Function |  |
| `PdxAccount.GetThirdPartyProviderLocalizationKey` | CString | Function |  |
| `PdxAccount.GetThirdPartyProviderLocalizedName` | CString | Function |  |
| `PdxAccount.IsLinkingPossible` | bool | Function |  |
| `PdxAccount.IsLoggedIn` | bool | Function |  |
| `PdxAccount.Self` | [unregistered] | Function |  |
| `PdxClearEditBoxText( Arg0 )` | void | Global function |  |
| `PdxGetWidgetScreenSize( Arg0 )` | CVector2f | Global function |  |
| `PdxGuiDestroyWidget( Arg0 )` | void | Global function | Destroy widget |
| `PdxGuiEditboxGetText( Arg0 )` | CUTF8String | Global function |  |
| `PdxGuiFoldOut` |  | Type | Foldable Item |
| `PdxGuiFoldOut.AccessSelf` | [unregistered] | Function |  |
| `PdxGuiFoldOut.Fold` | void | Function |  |
| `PdxGuiFoldOut.IsFolded` | bool | Function |  |
| `PdxGuiFoldOut.IsUnfolded` | bool | Function |  |
| `PdxGuiFoldOut.Self` | [unregistered] | Function |  |
| `PdxGuiFoldOut.SetFoldState` | void | Function |  |
| `PdxGuiFoldOut.Toggle` | void | Function |  |
| `PdxGuiFoldOut.Unfold` | void | Function |  |
| `PdxGuiGfxVideoControl` |  | Type |  |
| `PdxGuiGfxVideoControl.AccessSelf` | [unregistered] | Function |  |
| `PdxGuiGfxVideoControl.IsLooping` | bool | Function |  |
| `PdxGuiGfxVideoControl.IsPlaybackComplete` | bool | Function |  |
| `PdxGuiGfxVideoControl.IsPlaying` | bool | Function |  |
| `PdxGuiGfxVideoControl.Restart` | void | Function |  |
| `PdxGuiGfxVideoControl.Self` | [unregistered] | Function |  |
| `PdxGuiGfxVideoControl.SetLooping( Arg0 )` | void | Function |  |
| `PdxGuiGfxVideoControl.Start` | void | Function |  |
| `PdxGuiGfxVideoControl.Stop` | void | Function |  |
| `PdxGuiTableRow` |  | Type | Generic Tree Table - Row |
| `PdxGuiTableRow.AccessSelf` | [unregistered] | Function |  |
| `PdxGuiTableRow.GetIndex` | int32 | Function |  |
| `PdxGuiTableRow.Self` | [unregistered] | Function |  |
| `PdxGuiTreeTable` |  | Type | Generic Tree-Table |
| `PdxGuiTreeTable.AccessSelf` | [unregistered] | Function |  |
| `PdxGuiTreeTable.CanExpand( Arg0 )` | bool | Function |  |
| `PdxGuiTreeTable.Expand( Arg0 )` | void | Function |  |
| `PdxGuiTreeTable.GetDepth( Arg0 )` | int32 | Function |  |
| `PdxGuiTreeTable.GetIndent( Arg0, Arg1 )` | CVector2f | Function |  |
| `PdxGuiTreeTable.GetNumChildren( Arg0 )` | int32 | Function |  |
| `PdxGuiTreeTable.IsExpanded( Arg0 )` | bool | Function |  |
| `PdxGuiTreeTable.Self` | [unregistered] | Function |  |
| `PdxGuiTreeTable.ToggleExpand( Arg0 )` | void | Function |  |
| `PdxGuiTreeTable.Unexpand( Arg0 )` | void | Function |  |
| `PdxGuiWidget` |  | Type | GUI Widget |
| `PdxGuiWidget.AccessChild( Arg0 )` | PdxGuiWidget | Promote |  |
| `PdxGuiWidget.AccessParent` | PdxGuiWidget | Promote |  |
| `PdxGuiWidget.AccessParent` | [unregistered] | Function |  |
| `PdxGuiWidget.AccessSelf` | [unregistered] | Function |  |
| `PdxGuiWidget.ClearDLCMapModeOverride` | void | Function |  |
| `PdxGuiWidget.ClearHoveredUnits` | void | Function |  |
| `PdxGuiWidget.CloseContextMenu` | void | Function |  |
| `PdxGuiWidget.CountVisibleChildren` | int32 | Function |  |
| `PdxGuiWidget.FindChild( Arg0 )` | PdxGuiWidget | Promote |  |
| `PdxGuiWidget.FindChild( Arg0 )` | [unregistered] | Function |  |
| `PdxGuiWidget.FindParent( Arg0 )` | PdxGuiWidget | Promote |  |
| `PdxGuiWidget.FindParent( Arg0 )` | [unregistered] | Function |  |
| `PdxGuiWidget.GetAlpha` | float | Function |  |
| `PdxGuiWidget.GetCenterX` | float | Function |  |
| `PdxGuiWidget.GetCenterY` | float | Function |  |
| `PdxGuiWidget.GetChildrenCount` | int32 | Function |  |
| `PdxGuiWidget.GetIndexInDataModel` | int32 | Function |  |
| `PdxGuiWidget.GetIndexInParent` | int32 | Function |  |
| `PdxGuiWidget.GetName` | CString | Function |  |
| `PdxGuiWidget.GetOutlinerEntryDebug` | CString | Function |  |
| `PdxGuiWidget.GetPercentPositionFrom( Arg0, Arg1 )` | CVector2f | Function |  |
| `PdxGuiWidget.GetRawShortcut` | CUTF8String | Function |  |
| `PdxGuiWidget.GetRemainingHeightInParent( Arg0, Arg1 )` | float | Function |  |
| `PdxGuiWidget.GetShortcut` | CUTF8String | Function |  |
| `PdxGuiWidget.GetSizeX` | float | Function |  |
| `PdxGuiWidget.GetSizeY` | float | Function |  |
| `PdxGuiWidget.GetTooltipLockProgress` | float | Function |  |
| `PdxGuiWidget.GetTooltipMetaGameConcept` | CString | Function |  |
| `PdxGuiWidget.GetTooltipMetaTexture` | [unregistered] | Function |  |
| `PdxGuiWidget.GetTooltipMetaTitle` | CString | Function |  |
| `PdxGuiWidget.GfxVideoControl` | PdxGuiGfxVideoControl | Promote |  |
| `PdxGuiWidget.HasListFromTextContext` | bool | Function |  |
| `PdxGuiWidget.HasTooltipMeta` | bool | Function |  |
| `PdxGuiWidget.HasTooltipMetaGameConcept` | bool | Function |  |
| `PdxGuiWidget.HasTooltipMetaTitle` | bool | Function |  |
| `PdxGuiWidget.HasUIAction` | bool | Function |  |
| `PdxGuiWidget.Hide` | void | Function |  |
| `PdxGuiWidget.InterruptAllChildrenAnimation( Arg0 )` | void | Function |  |
| `PdxGuiWidget.InterruptAnimation( Arg0 )` | void | Function |  |
| `PdxGuiWidget.InterruptThenTriggerAllChildrenAnimation( Arg0, Arg1 )` | void | Function |  |
| `PdxGuiWidget.InterruptThenTriggerAnimation( Arg0, Arg1 )` | void | Function |  |
| `PdxGuiWidget.IsEnabled` | bool | Function |  |
| `PdxGuiWidget.IsIndexInParentLast` | bool | Function |  |
| `PdxGuiWidget.IsKeyHoveredByWidgetName` | bool | Function |  |
| `PdxGuiWidget.IsMouseOver` | bool | Function |  |
| `PdxGuiWidget.IsNestedTooltip` | bool | Function |  |
| `PdxGuiWidget.IsTextContextEmpty` | bool | Function |  |
| `PdxGuiWidget.IsTooltipLocked` | bool | Function |  |
| `PdxGuiWidget.IsUberButtonEnabled` | bool | Function |  |
| `PdxGuiWidget.IsUberButtonVisible` | bool | Function |  |
| `PdxGuiWidget.IsVisible` | bool | Function |  |
| `PdxGuiWidget.KeepRatioFromHeightPercent( Arg0, Arg1 )` | CVector2f | Function |  |
| `PdxGuiWidget.KeepRatioFromWidthPercent( Arg0, Arg1 )` | CVector2f | Function |  |
| `PdxGuiWidget.NamePathContains( Arg0 )` | bool | Function |  |
| `PdxGuiWidget.Parent` | [unregistered] | Function |  |
| `PdxGuiWidget.PopMapModeOverride` | void | Function |  |
| `PdxGuiWidget.PushDLCMapModeOverride( Arg0 )` | void | Function |  |
| `PdxGuiWidget.PushMapModeOverride( Arg0 )` | void | Function |  |
| `PdxGuiWidget.Self` | [unregistered] | Function |  |
| `PdxGuiWidget.SetAlertSmoothPositioning( Arg0 )` | void | Function |  |
| `PdxGuiWidget.SetBattleSubUnitHover( Arg0 )` | void | Function |  |
| `PdxGuiWidget.SetHighlightArea( Arg0 )` | void | Function |  |
| `PdxGuiWidget.SetHighlightBuilding( Arg0 )` | void | Function |  |
| `PdxGuiWidget.SetHighlightCabinetActionTarget( Arg0 )` | void | Function |  |
| `PdxGuiWidget.SetHighlightCharacter( Arg0 )` | void | Function |  |
| `PdxGuiWidget.SetHighlightCountry( Arg0 )` | void | Function |  |
| `PdxGuiWidget.SetHighlightCulture( Arg0 )` | void | Function |  |
| `PdxGuiWidget.SetHighlightCultureGroup( Arg0 )` | void | Function |  |
| `PdxGuiWidget.SetHighlightDialect( Arg0 )` | void | Function |  |
| `PdxGuiWidget.SetHighlightDisease( Arg0 )` | void | Function |  |
| `PdxGuiWidget.SetHighlightDiseaseOutbreak( Arg0 )` | void | Function |  |
| `PdxGuiWidget.SetHighlightDynasty( Arg0 )` | void | Function |  |
| `PdxGuiWidget.SetHighlightFort( Arg0 )` | void | Function |  |
| `PdxGuiWidget.SetHighlightGoods( Arg0 )` | void | Function |  |
| `PdxGuiWidget.SetHighlightInstitution( Arg0 )` | void | Function |  |
| `PdxGuiWidget.SetHighlightInternationalOrganization( Arg0 )` | void | Function |  |
| `PdxGuiWidget.SetHighlightLanguage( Arg0 )` | void | Function |  |
| `PdxGuiWidget.SetHighlightLanguageFamily( Arg0 )` | void | Function |  |
| `PdxGuiWidget.SetHighlightLedger( Arg0 )` | void | Function |  |
| `PdxGuiWidget.SetHighlightLocation( Arg0 )` | void | Function |  |
| `PdxGuiWidget.SetHighlightLocationList( Arg0 )` | void | Function |  |
| `PdxGuiWidget.SetHighlightLocations( Arg0 )` | void | Function |  |
| `PdxGuiWidget.SetHighlightLocationSet( Arg0 )` | void | Function |  |
| `PdxGuiWidget.SetHighlightLocationsRef( Arg0 )` | void | Function |  |
| `PdxGuiWidget.SetHighlightMarket( Arg0 )` | void | Function |  |
| `PdxGuiWidget.SetHighlightMercenary( Arg0 )` | void | Function |  |
| `PdxGuiWidget.SetHighlightObjectiveGeography( Arg0 )` | void | Function |  |
| `PdxGuiWidget.SetHighlightOwnGoods( Arg0 )` | void | Function |  |
| `PdxGuiWidget.SetHighlightPrivateer( Arg0 )` | void | Function |  |
| `PdxGuiWidget.SetHighlightProvince( Arg0 )` | void | Function |  |
| `PdxGuiWidget.SetHighlightProvinceDefinition( Arg0 )` | void | Function |  |
| `PdxGuiWidget.SetHighlightRebel( Arg0 )` | void | Function |  |
| `PdxGuiWidget.SetHighlightRegion( Arg0 )` | void | Function |  |
| `PdxGuiWidget.SetHighlightReligion( Arg0 )` | void | Function |  |
| `PdxGuiWidget.SetHighlightReligionGroup( Arg0 )` | void | Function |  |
| `PdxGuiWidget.SetHighlightReligiousOrderLocations( Arg0 )` | void | Function |  |
| `PdxGuiWidget.SetHighlightTrade( Arg0 )` | void | Function |  |
| `PdxGuiWidget.SetHighlightUnitAbility( Arg0 )` | void | Function |  |
| `PdxGuiWidget.SetHighlightWar( Arg0 )` | void | Function |  |
| `PdxGuiWidget.SetHighlightWarGoal( Arg0 )` | void | Function |  |
| `PdxGuiWidget.SetHoveredUnit( Arg0 )` | void | Function |  |
| `PdxGuiWidget.SetTradeLinesMarket( Arg0 )` | void | Function |  |
| `PdxGuiWidget.SetTradeLinesTrade( Arg0 )` | void | Function |  |
| `PdxGuiWidget.SetTradeLinesTradeOrder( Arg0 )` | void | Function |  |
| `PdxGuiWidget.SetVisible( Arg0 )` | void | Function | Sets the visibility of the PdxGuiWidget based on a provided condition |
| `PdxGuiWidget.Show` | void | Function |  |
| `PdxGuiWidget.StackBottom` | void | Function |  |
| `PdxGuiWidget.StackTop` | void | Function |  |
| `PdxGuiWidget.ToggleContextMenu` | void | Function |  |
| `PdxGuiWidget.ToggleVisibility` | void | Function | Toggles the visibility of the PdxGuiWidget |
| `PdxGuiWidget.TriggerAllChildrenAnimation( Arg0 )` | void | Function |  |
| `PdxGuiWidget.TriggerAnimation( Arg0 )` | void | Function |  |
| `PlayerIsHost` | bool | Global function | Multiplayer GUI - Is the current player the host? |
| `PlayerJoinRequest` |  | Type | Jomini Multiplayer - Player Join Request |
| `PlayerJoinRequest.Accept` | void | Function |  |
| `PlayerJoinRequest.AccessSelf` | [unregistered] | Function |  |
| `PlayerJoinRequest.GetPlayerName` | CString | Function |  |
| `PlayerJoinRequest.IsAccepted` | bool | Function |  |
| `PlayerJoinRequest.IsRejected` | bool | Function |  |
| `PlayerJoinRequest.Reject` | void | Function |  |
| `PlayerJoinRequest.Self` | [unregistered] | Function |  |
| `PlayerJoinRequest.SetAccepted` | void | Function |  |
| `PlayerJoinRequest.SetRejected` | void | Function |  |
| `PlotLine` |  | Type |  |
| `PlotLine.AccessSelf` | [unregistered] | Function |  |
| `PlotLine.GetInterpolatedIndexNearMouse` | float | Function |  |
| `PlotLine.GetInterpolatedValueNearMouse` | CVector2f | Function |  |
| `PlotLine.GetPlotRect` | CPdxFloatRect | Function |  |
| `PlotLine.GetSnappedIndexNearMouse` | int32 | Function |  |
| `PlotLine.GetSnappedValueNearMouse` | CVector2f | Function |  |
| `PlotLine.HasPositionNearMouse` | bool | Function |  |
| `PlotLine.Self` | [unregistered] | Function |  |
| `PortraitTooltip` |  | Type | Portrait Tooltip |
| `PortraitTooltip.AccessSelf` | [unregistered] | Function |  |
| `PortraitTooltip.Self` | [unregistered] | Function |  |
| `RemoveFriendConfirmWindow` |  | Type | Jomini Friend List - Remove friend confirm window |
| `RemoveFriendConfirmWindow.AccessSelf` | [unregistered] | Function |  |
| `RemoveFriendConfirmWindow.ConfirmRemove` | void | Function |  |
| `RemoveFriendConfirmWindow.Hide` | void | Function |  |
| `RemoveFriendConfirmWindow.Self` | [unregistered] | Function |  |
| `RemoveFriendConfirmWindow.ShouldBeShown` | bool | Function |  |
| `RemoveFriendConfirmWindow.Show` | void | Function |  |
| `ScopedEditorSettingsCategory` |  | Type |  |
| `ScopedEditorSettingsCategory.AccessCategory` | EditorSettingCategory | Promote |  |
| `ScopedEditorSettingsCategory.AccessSelf` | [unregistered] | Function |  |
| `ScopedEditorSettingsCategory.GetCategory` | EditorSettingCategory | Promote |  |
| `ScopedEditorSettingsCategory.Self` | [unregistered] | Function |  |
| `SelectEnumWithString( Arg0, Arg1 )` | void | Global function | Settings - Select enum by string input |
| `ServerInformation` |  | Type |  |
| `ServerInformation.AccessSelf` | [unregistered] | Function |  |
| `ServerInformation.GetMPMode` | CString | Function |  |
| `ServerInformation.GetServerID` | CUTF8String | Function |  |
| `ServerInformation.GetServerName` | CUTF8String | Function |  |
| `ServerInformation.HasServerID` | bool | Function |  |
| `ServerInformation.HasServerName` | bool | Function |  |
| `ServerInformation.IsInvite` | bool | Function |  |
| `ServerInformation.IsPublic` | bool | Function |  |
| `ServerInformation.IsSingleplayer` | bool | Function |  |
| `ServerInformation.Self` | [unregistered] | Function |  |
| `SettingCategory` |  | Type | Jomini Settings - Category |
| `SettingCategory.AccessSelf` | [unregistered] | Function |  |
| `SettingCategory.AccessSettingsOrder` | [unregistered] | Function |  |
| `SettingCategory.GetName` | CUTF8String | Function |  |
| `SettingCategory.GetSettingsOrder` | [unregistered] | Function |  |
| `SettingCategory.HasSettingsWidgets` | bool | Function |  |
| `SettingCategory.Self` | [unregistered] | Function |  |
| `SettingsPage` |  | Type | Jomini Settings - Settings Page |
| `SettingsPage.AccessCategories` | [unregistered] | Function |  |
| `SettingsPage.AccessSelf` | [unregistered] | Function |  |
| `SettingsPage.GetName` | CUTF8String | Function |  |
| `SettingsPage.IsSelected` | bool | Function |  |
| `SettingsPage.OnClick` | void | Function |  |
| `SettingsPage.Self` | [unregistered] | Function |  |
| `ShouldShowSegmentedControlForSetting( Arg0 )` | bool | Global function | Settings - Should show segmented control or not? |
| `ShowExpirationDateInVariableListForTarget( Arg0, Arg1, Arg2 )` | CString | Global function | Show the expiration date of a variable from a variable list. |
| `ShowSimpleCustomTooltip( Arg0, Arg1, Arg2, Arg3, Arg4, Arg5, Arg6 )` | CString | Global function | Creates a simple custom TagTooltip for the string in the first argument. Other arguments are the loc-key for t… |
| `ShowSimpleCustomTooltipLocalized( Arg0, Arg1, Arg2, Arg3, Arg4, Arg5, Arg6 )` | CString | Global function | Like ShowSimpleCustomTooltip, except the string in the first argument gets localized. |
| `Social` |  | Type | Jomini Social Manager |
| `Social.AccessSelf` | [unregistered] | Function |  |
| `Social.GetTotalUnreadChatMessages` | int32 | Function |  |
| `Social.Self` | [unregistered] | Function |  |
| `SocialNotificationWindow` |  | Type | Jomini Friend List - Notification window |
| `SocialNotificationWindow.AccessChatMessages` | [unregistered] | Function |  |
| `SocialNotificationWindow.AccessSelf` | [unregistered] | Function |  |
| `SocialNotificationWindow.GetNotificationsLayoutAnchor` | [unregistered] | Function |  |
| `SocialNotificationWindow.Hide` | void | Function |  |
| `SocialNotificationWindow.Self` | [unregistered] | Function |  |
| `SocialNotificationWindow.ShouldBeShown` | bool | Function |  |
| `SocialNotificationWindow.Show` | void | Function |  |
| `SocialUI` |  | Type | Jomini Friend List - Social UI |
| `SocialUI.AccessSelf` | [unregistered] | Function |  |
| `SocialUI.Hide` | void | Function |  |
| `SocialUI.IsAllowedToBeShown` | bool | Function |  |
| `SocialUI.Self` | [unregistered] | Function |  |
| `SocialUI.Show` | void | Function |  |
| `SocialUI.ToggleShow` | void | Function |  |
| `SocialUI.WantsToBeShown` | bool | Function |  |
| `SocialWidget` |  | Type | Jomini Social Widget |
| `SocialWidget.AccessSelf` | [unregistered] | Function |  |
| `SocialWidget.Self` | [unregistered] | Function |  |
| `StatusCanLogin` | bool | Global function |  |
| `StatusGetLoginStatus` | CUTF8String | Global function |  |
| `StatusGetUserEmailMasked` | CUTF8String | Global function |  |
| `StatusGetUserName` | CUTF8String | Global function |  |
| `StatusIsAccountConnected` | bool | Global function |  |
| `StatusIsLoggedIn` | bool | Global function |  |
| `StatusIsLoggingIn` | bool | Global function |  |
| `StatusIsOffline` | bool | Global function |  |
| `StatusIsSupportConnectedAccount` | bool | Global function |  |
| `StatusIsUserNameEmpty` | bool | Global function |  |
| `StatusWidget` |  | Type | Status Widget |
| `StatusWidget.AccessSelf` | [unregistered] | Function |  |
| `StatusWidget.ConnectAccount` | void | Function |  |
| `StatusWidget.DisconnectAccount` | void | Function |  |
| `StatusWidget.Hide( Arg0 )` | void | Function |  |
| `StatusWidget.IsAccountConnected` | bool | Function |  |
| `StatusWidget.IsBusy` | bool | Function |  |
| `StatusWidget.Logout` | void | Function |  |
| `StatusWidget.Self` | [unregistered] | Function |  |
| `StatusWidget.Show( Arg0 )` | void | Function |  |
| `StatusWidget.SupportsConnectAccount` | bool | Function |  |
| `StatusWidget.ToggleCreateAccountWindow` | void | Function |  |
| `StatusWidget.ToggleCreateProfileWindow` | void | Function |  |
| `StatusWidget.ToggleLoginWindow` | void | Function |  |
| `Unfocus( Arg0 )` | void | Global function | Unfocus widget |
| `UniversalSplineEditorPropertyCategory` |  | Type | Universal spline editor - Property category |
| `UniversalSplineEditorPropertyCategory.AccessSelf` | [unregistered] | Function |  |
| `UniversalSplineEditorPropertyCategory.GetName` | CString | Function |  |
| `UniversalSplineEditorPropertyCategory.IsEmpty` | bool | Function |  |
| `UniversalSplineEditorPropertyCategory.IsExpanded` | bool | Function |  |
| `UniversalSplineEditorPropertyCategory.Self` | [unregistered] | Function |  |
| `UniversalSplineEditorPropertyCategory.ToggleExpanded` | void | Function |  |

## data_types_script.txt（956 条）

| 名称 | 返回类型 | 定义类型 | 说明 |
|---|---|---|---|
| `ActiveResolution` |  | Type |  |
| `ActiveResolution.MakeScope` | Scope | Function | Jomini Script System |
| `AdvanceDefinition` |  | Type |  |
| `AdvanceDefinition.MakeScope` | Scope | Function | Jomini Script System |
| `Age` |  | Type |  |
| `Age.MakeScope` | Scope | Function | Jomini Script System |
| `Ambition` |  | Type |  |
| `Ambition.MakeScope` | Scope | Function | Jomini Script System |
| `AmbitionDefinition` |  | Type |  |
| `AmbitionDefinition.MakeScope` | Scope | Function | Jomini Script System |
| `Area` |  | Type |  |
| `Area.MakeScope` | Scope | Function | Jomini Script System |
| `Artist` |  | Type |  |
| `Artist.MakeScope` | Scope | Function | Jomini Script System |
| `Avatar` |  | Type |  |
| `Avatar.MakeScope` | Scope | Function | Jomini Script System |
| `Building` |  | Type |  |
| `Building.MakeScope` | Scope | Function | Jomini Script System |
| `BuildingType` |  | Type |  |
| `BuildingType.MakeScope` | Scope | Function | Jomini Script System |
| `Bureaucracy` |  | Type |  |
| `Bureaucracy.MakeScope` | Scope | Function | Jomini Script System |
| `BureaucracyType` |  | Type |  |
| `BureaucracyType.MakeScope` | Scope | Function | Jomini Script System |
| `Cabinet` |  | Type |  |
| `Cabinet.MakeScope` | Scope | Function | Jomini Script System |
| `Calling` |  | Type |  |
| `Calling.MakeScope` | Scope | Function | Jomini Script System |
| `CasusBelli` |  | Type |  |
| `CasusBelli.MakeScope` | Scope | Function | Jomini Script System |
| `Character` |  | Type |  |
| `Character.MakeScope` | Scope | Function | Jomini Script System |
| `CharacterInteraction` |  | Type |  |
| `CharacterInteraction.MakeScope` | Scope | Function | Jomini Script System |
| `ChildEducation` |  | Type |  |
| `ChildEducation.MakeScope` | Scope | Function | Jomini Script System |
| `ChivalricOrder` |  | Type |  |
| `ChivalricOrder.MakeScope` | Scope | Function | Jomini Script System |
| `Climate` |  | Type |  |
| `Climate.MakeScope` | Scope | Function | Jomini Script System |
| `ColonialCharter` |  | Type |  |
| `ColonialCharter.MakeScope` | Scope | Function | Jomini Script System |
| `Combat` |  | Type |  |
| `Combat.MakeScope` | Scope | Function | Jomini Script System |
| `CombatSide` |  | Type |  |
| `CombatSide.MakeScope` | Scope | Function | Jomini Script System |
| `Container` |  | Type |  |
| `Container.AccessSelf` | [unregistered] | Function |  |
| `Container.Self` | [unregistered] | Function |  |
| `Continent` |  | Type |  |
| `Continent.MakeScope` | Scope | Function | Jomini Script System |
| `Country` |  | Type |  |
| `Country.MakeScope` | Scope | Function | Jomini Script System |
| `CountryInteraction` |  | Type |  |
| `CountryInteraction.MakeScope` | Scope | Function | Jomini Script System |
| `CountryRank` |  | Type |  |
| `CountryRank.MakeScope` | Scope | Function | Jomini Script System |
| `Culture` |  | Type |  |
| `Culture.MakeScope` | Scope | Function | Jomini Script System |
| `CultureGroup` |  | Type |  |
| `CultureGroup.MakeScope` | Scope | Function | Jomini Script System |
| `DATE` | Date | Global promote | Jomini Date |
| `DATE_MAX` | Date | Global promote | Jomini Date - Max |
| `DATE_MIN` | Date | Global promote | Jomini Date - Min |
| `DisasterType` |  | Type |  |
| `DisasterType.MakeScope` | Scope | Function | Jomini Script System |
| `Disease` |  | Type |  |
| `Disease.MakeScope` | Scope | Function | Jomini Script System |
| `DiseaseOutbreak` |  | Type |  |
| `DiseaseOutbreak.MakeScope` | Scope | Function | Jomini Script System |
| `Dynasty` |  | Type |  |
| `Dynasty.MakeScope` | Scope | Function | Jomini Script System |
| `EmploymentSystem` |  | Type |  |
| `EmploymentSystem.MakeScope` | Scope | Function | Jomini Script System |
| `EmptyScope` | TopScope | Global promote |  |
| `Estate` |  | Type |  |
| `Estate.MakeScope` | Scope | Function | Jomini Script System |
| `EstateType` |  | Type |  |
| `EstateType.MakeScope` | Scope | Function | Jomini Script System |
| `Ethnicity` |  | Type | Portrait System - Ethnicity |
| `Ethnicity.MakeScope` | Scope | Function | Jomini Script System |
| `Exploration` |  | Type |  |
| `Exploration.MakeScope` | Scope | Function | Jomini Script System |
| `FormableCountry` |  | Type |  |
| `FormableCountry.MakeScope` | Scope | Function | Jomini Script System |
| `GenericAction` |  | Type |  |
| `GenericAction.MakeScope` | Scope | Function | Jomini Script System |
| `GetGlobalList( Arg0 )` | [unregistered] | Global function |  |
| `GetGlobalVariable( Arg0 )` | Scope | Global promote |  |
| `GetGlobalVariable( Arg0 )` | [unregistered] | Global function |  |
| `GetGlobalVariableListTicks( Arg0, Arg1 )` | int32 | Global function |  |
| `GetGlobalVariableMapTicks( Arg0, Arg1 )` | int32 | Global function |  |
| `GetGlobalVariableTicks( Arg0 )` | int32 | Global function |  |
| `GetVariableFromGlobalVariableMap( Arg0, Arg1 )` | [unregistered] | Global function |  |
| `GetVariableFromGlobalVariableMap( Arg0, Arg1 )` | Scope | Global promote |  |
| `GetVarTimeRemaining( Arg0, Arg1 )` | int32 | Global function |  |
| `God` |  | Type |  |
| `God.MakeScope` | Scope | Function | Jomini Script System |
| `Goods` |  | Type |  |
| `Goods.MakeScope` | Scope | Function | Jomini Script System |
| `GoodsDemand` |  | Type |  |
| `GoodsDemand.MakeScope` | Scope | Function | Jomini Script System |
| `GovernmentType` |  | Type |  |
| `GovernmentType.MakeScope` | Scope | Function | Jomini Script System |
| `Hegemony` |  | Type |  |
| `Hegemony.MakeScope` | Scope | Function | Jomini Script System |
| `HeirSelection` |  | Type |  |
| `HeirSelection.MakeScope` | Scope | Function | Jomini Script System |
| `HolySite` |  | Type |  |
| `HolySite.MakeScope` | Scope | Function | Jomini Script System |
| `HolySiteDefinition` |  | Type |  |
| `HolySiteDefinition.MakeScope` | Scope | Function | Jomini Script System |
| `HolySiteType` |  | Type |  |
| `HolySiteType.MakeScope` | Scope | Function | Jomini Script System |
| `ImperialCircle` |  | Type |  |
| `ImperialCircle.MakeScope` | Scope | Function | Jomini Script System |
| `Implementable` |  | Type | Base class for implementable game structures |
| `Implementable.AccessSelf` | [unregistered] | Function |  |
| `Implementable.HasPotential` | bool | Function |  |
| `Implementable.Self` | [unregistered] | Function |  |
| `IndustryPromotion` |  | Type |  |
| `IndustryPromotion.MakeScope` | Scope | Function | Jomini Script System |
| `IndustryType` |  | Type |  |
| `IndustryType.MakeScope` | Scope | Function | Jomini Script System |
| `Institution` |  | Type |  |
| `Institution.MakeScope` | Scope | Function | Jomini Script System |
| `InternationalOrganization` |  | Type |  |
| `InternationalOrganization.MakeScope` | Scope | Function | Jomini Script System |
| `InternationalOrganizationType` |  | Type |  |
| `InternationalOrganizationType.MakeScope` | Scope | Function | Jomini Script System |
| `LandOwnershipRule` |  | Type |  |
| `LandOwnershipRule.MakeScope` | Scope | Function | Jomini Script System |
| `Language` |  | Type |  |
| `Language.MakeScope` | Scope | Function | Jomini Script System |
| `LanguageFamily` |  | Type |  |
| `LanguageFamily.MakeScope` | Scope | Function | Jomini Script System |
| `Law` |  | Type |  |
| `Law.MakeScope` | Scope | Function | Jomini Script System |
| `LevySetup` |  | Type |  |
| `LevySetup.MakeScope` | Scope | Function | Jomini Script System |
| `Loan` |  | Type |  |
| `Loan.MakeScope` | Scope | Function | Jomini Script System |
| `Location` |  | Type |  |
| `Location.MakeScope` | Scope | Function | Jomini Script System |
| `LocationRank` |  | Type |  |
| `LocationRank.MakeScope` | Scope | Function | Jomini Script System |
| `MakeScopeBool( Arg0 )` | Scope | Global function |  |
| `MakeScopeFlag( Arg0 )` | Scope | Global function |  |
| `MakeScopeValue( Arg0 )` | Scope | Global function |  |
| `Market` |  | Type |  |
| `Market.MakeScope` | Scope | Function | Jomini Script System |
| `Mercenary` |  | Type |  |
| `Mercenary.MakeScope` | Scope | Function | Jomini Script System |
| `MissionDefinition` |  | Type |  |
| `MissionDefinition.MakeScope` | Scope | Function | Jomini Script System |
| `MissionTaskDefinition` |  | Type |  |
| `MissionTaskDefinition.MakeScope` | Scope | Function | Jomini Script System |
| `Movement` |  | Type |  |
| `Movement.MakeScope` | Scope | Function | Jomini Script System |
| `MovementDefinition` |  | Type |  |
| `MovementDefinition.MakeScope` | Scope | Function | Jomini Script System |
| `Omen` |  | Type |  |
| `Omen.MakeScope` | Scope | Function | Jomini Script System |
| `ParliamentAgenda` |  | Type |  |
| `ParliamentAgenda.MakeScope` | Scope | Function | Jomini Script System |
| `ParliamentIssue` |  | Type |  |
| `ParliamentIssue.MakeScope` | Scope | Function | Jomini Script System |
| `ParliamentType` |  | Type |  |
| `ParliamentType.MakeScope` | Scope | Function | Jomini Script System |
| `Payment` |  | Type |  |
| `Payment.MakeScope` | Scope | Function | Jomini Script System |
| `Policy` |  | Type |  |
| `Policy.MakeScope` | Scope | Function | Jomini Script System |
| `Pop` |  | Type |  |
| `Pop.MakeScope` | Scope | Function | Jomini Script System |
| `PopType` |  | Type |  |
| `PopType.MakeScope` | Scope | Function | Jomini Script System |
| `PREV` | Scope | Global promote | Script Scope |
| `Price` |  | Type |  |
| `Price.MakeScope` | Scope | Function | Jomini Script System |
| `Privateer` |  | Type |  |
| `Privateer.MakeScope` | Scope | Function | Jomini Script System |
| `ProductionMethod` |  | Type |  |
| `ProductionMethod.MakeScope` | Scope | Function | Jomini Script System |
| `Province` |  | Type |  |
| `Province.MakeScope` | Scope | Function | Jomini Script System |
| `ProvinceDefinition` |  | Type |  |
| `ProvinceDefinition.MakeScope` | Scope | Function | Jomini Script System |
| `Rebel` |  | Type |  |
| `Rebel.MakeScope` | Scope | Function | Jomini Script System |
| `RecruitmentMethod` |  | Type |  |
| `RecruitmentMethod.MakeScope` | Scope | Function | Jomini Script System |
| `RegencyType` |  | Type |  |
| `RegencyType.MakeScope` | Scope | Function | Jomini Script System |
| `Region` |  | Type |  |
| `Region.MakeScope` | Scope | Function | Jomini Script System |
| `Religion` |  | Type |  |
| `Religion.MakeScope` | Scope | Function | Jomini Script System |
| `ReligionGroup` |  | Type |  |
| `ReligionGroup.MakeScope` | Scope | Function | Jomini Script System |
| `ReligiousAspect` |  | Type |  |
| `ReligiousAspect.MakeScope` | Scope | Function | Jomini Script System |
| `ReligiousFaction` |  | Type |  |
| `ReligiousFaction.MakeScope` | Scope | Function | Jomini Script System |
| `ReligiousFigure` |  | Type |  |
| `ReligiousFigure.MakeScope` | Scope | Function | Jomini Script System |
| `ReligiousFocus` |  | Type |  |
| `ReligiousFocus.MakeScope` | Scope | Function | Jomini Script System |
| `ReligiousOrder` |  | Type |  |
| `ReligiousOrder.MakeScope` | Scope | Function | Jomini Script System |
| `ReligiousOrderType` |  | Type |  |
| `ReligiousOrderType.MakeScope` | Scope | Function | Jomini Script System |
| `ReligiousSchool` |  | Type |  |
| `ReligiousSchool.MakeScope` | Scope | Function | Jomini Script System |
| `Resolution` |  | Type |  |
| `Resolution.MakeScope` | Scope | Function | Jomini Script System |
| `RoadType` |  | Type |  |
| `RoadType.MakeScope` | Scope | Function | Jomini Script System |
| `ROOT` | Scope | Global promote | Script Scope |
| `Scope` |  | Type |  |
| `SCOPE` | TopScope | Global promote | Script Scope |
| `Scope.AccessSelf` | [unregistered] | Function |  |
| `Scope.ActiveResolution` | [unregistered] | Function | Jomini Script System |
| `Scope.ActiveResolution` | ActiveResolution | Promote | Jomini Script System |
| `Scope.AdvanceType` | AdvanceDefinition | Promote | Jomini Script System |
| `Scope.AdvanceType` | [unregistered] | Function | Jomini Script System |
| `Scope.Age` | [unregistered] | Function | Jomini Script System |
| `Scope.Age` | Age | Promote | Jomini Script System |
| `Scope.Ambition` | Ambition | Promote | Jomini Script System |
| `Scope.Ambition` | [unregistered] | Function | Jomini Script System |
| `Scope.AmbitionDefinition` | [unregistered] | Function | Jomini Script System |
| `Scope.AmbitionDefinition` | AmbitionDefinition | Promote | Jomini Script System |
| `Scope.Area` | [unregistered] | Function | Jomini Script System |
| `Scope.Area` | Area | Promote | Jomini Script System |
| `Scope.Artist` | [unregistered] | Function | Jomini Script System |
| `Scope.Artist` | Artist | Promote | Jomini Script System |
| `Scope.Avatar` | [unregistered] | Function | Jomini Script System |
| `Scope.Avatar` | Avatar | Promote | Jomini Script System |
| `Scope.Building` | Building | Promote | Jomini Script System |
| `Scope.Building` | [unregistered] | Function | Jomini Script System |
| `Scope.BuildingType` | [unregistered] | Function | Jomini Script System |
| `Scope.BuildingType` | BuildingType | Promote | Jomini Script System |
| `Scope.Bureaucracy` | Bureaucracy | Promote | Jomini Script System |
| `Scope.Bureaucracy` | [unregistered] | Function | Jomini Script System |
| `Scope.BureaucracyType` | BureaucracyType | Promote | Jomini Script System |
| `Scope.BureaucracyType` | [unregistered] | Function | Jomini Script System |
| `Scope.Cabinet` | [unregistered] | Function | Jomini Script System |
| `Scope.Cabinet` | Cabinet | Promote | Jomini Script System |
| `Scope.Calling` | Calling | Promote | Jomini Script System |
| `Scope.Calling` | [unregistered] | Function | Jomini Script System |
| `Scope.CasusBelli` | CasusBelli | Promote | Jomini Script System |
| `Scope.CasusBelli` | [unregistered] | Function | Jomini Script System |
| `Scope.Character` | Character | Promote | Jomini Script System |
| `Scope.Character` | [unregistered] | Function | Jomini Script System |
| `Scope.CharacterInteraction` | CharacterInteraction | Promote | Jomini Script System |
| `Scope.CharacterInteraction` | [unregistered] | Function | Jomini Script System |
| `Scope.ChildEducation` | ChildEducation | Promote | Jomini Script System |
| `Scope.ChildEducation` | [unregistered] | Function | Jomini Script System |
| `Scope.ChivalricOrder` | [unregistered] | Function | Jomini Script System |
| `Scope.ChivalricOrder` | ChivalricOrder | Promote | Jomini Script System |
| `Scope.Climate` | Climate | Promote | Jomini Script System |
| `Scope.Climate` | [unregistered] | Function | Jomini Script System |
| `Scope.ColonialCharter` | ColonialCharter | Promote | Jomini Script System |
| `Scope.ColonialCharter` | [unregistered] | Function | Jomini Script System |
| `Scope.Combat` | Combat | Promote | Jomini Script System |
| `Scope.Combat` | [unregistered] | Function | Jomini Script System |
| `Scope.CombatSide` | CombatSide | Promote | Jomini Script System |
| `Scope.CombatSide` | [unregistered] | Function | Jomini Script System |
| `Scope.Continent` | [unregistered] | Function | Jomini Script System |
| `Scope.Continent` | Continent | Promote | Jomini Script System |
| `Scope.Country` | [unregistered] | Function | Jomini Script System |
| `Scope.Country` | Country | Promote | Jomini Script System |
| `Scope.CountryInteraction` | CountryInteraction | Promote | Jomini Script System |
| `Scope.CountryInteraction` | [unregistered] | Function | Jomini Script System |
| `Scope.CountryRank` | [unregistered] | Function | Jomini Script System |
| `Scope.CountryRank` | CountryRank | Promote | Jomini Script System |
| `Scope.Culture` | Culture | Promote | Jomini Script System |
| `Scope.Culture` | [unregistered] | Function | Jomini Script System |
| `Scope.CultureGroup` | CultureGroup | Promote | Jomini Script System |
| `Scope.CultureGroup` | [unregistered] | Function | Jomini Script System |
| `Scope.DisasterType` | DisasterType | Promote | Jomini Script System |
| `Scope.DisasterType` | [unregistered] | Function | Jomini Script System |
| `Scope.Disease` | Disease | Promote | Jomini Script System |
| `Scope.Disease` | [unregistered] | Function | Jomini Script System |
| `Scope.DiseaseOutbreak` | [unregistered] | Function | Jomini Script System |
| `Scope.DiseaseOutbreak` | DiseaseOutbreak | Promote | Jomini Script System |
| `Scope.Dynasty` | Dynasty | Promote | Jomini Script System |
| `Scope.Dynasty` | [unregistered] | Function | Jomini Script System |
| `Scope.EmploymentSystem` | [unregistered] | Function | Jomini Script System |
| `Scope.EmploymentSystem` | EmploymentSystem | Promote | Jomini Script System |
| `Scope.Estate` | Estate | Promote | Jomini Script System |
| `Scope.Estate` | [unregistered] | Function | Jomini Script System |
| `Scope.EstateType` | [unregistered] | Function | Jomini Script System |
| `Scope.EstateType` | EstateType | Promote | Jomini Script System |
| `Scope.Ethnicity` | [unregistered] | Function | Jomini Script System |
| `Scope.Ethnicity` | Ethnicity | Promote | Jomini Script System |
| `Scope.Exploration` | [unregistered] | Function | Jomini Script System |
| `Scope.Exploration` | Exploration | Promote | Jomini Script System |
| `Scope.Faction` | [unregistered] | Function | Jomini Script System |
| `Scope.Faction` | ReligiousFaction | Promote | Jomini Script System |
| `Scope.FormableCountry` | [unregistered] | Function | Jomini Script System |
| `Scope.FormableCountry` | FormableCountry | Promote | Jomini Script System |
| `Scope.GenericAction` | GenericAction | Promote | Jomini Script System |
| `Scope.GenericAction` | [unregistered] | Function | Jomini Script System |
| `Scope.GetActiveResolution` | ActiveResolution | Promote |  |
| `Scope.GetAdvanceType` | AdvanceDefinition | Promote |  |
| `Scope.GetAge` | Age | Promote |  |
| `Scope.GetAmbition` | Ambition | Promote |  |
| `Scope.GetAmbitionDefinition` | AmbitionDefinition | Promote |  |
| `Scope.GetArea` | Area | Promote |  |
| `Scope.GetArtist` | Artist | Promote |  |
| `Scope.GetAvatar` | Avatar | Promote |  |
| `Scope.GetBuilding` | Building | Promote |  |
| `Scope.GetBuildingType` | BuildingType | Promote |  |
| `Scope.GetBureaucracy` | Bureaucracy | Promote |  |
| `Scope.GetBureaucracyType` | BureaucracyType | Promote |  |
| `Scope.GetCabinet` | Cabinet | Promote |  |
| `Scope.GetCabinetAction` | CabinetAction | Promote |  |
| `Scope.GetCasusBelli` | CasusBelli | Promote |  |
| `Scope.GetCharacter` | Character | Promote |  |
| `Scope.GetCharacterInteraction` | CharacterInteraction | Promote |  |
| `Scope.GetChildEducation` | ChildEducation | Promote |  |
| `Scope.GetChivalricOrder` | ChivalricOrder | Promote |  |
| `Scope.GetClimate` | Climate | Promote |  |
| `Scope.GetColonialCharter` | ColonialCharter | Promote |  |
| `Scope.GetColonialCharterGoal` | ColonialCharterGoalDefinition | Promote |  |
| `Scope.GetCombat` | Combat | Promote |  |
| `Scope.GetCombatSide` | CombatSide | Promote |  |
| `Scope.GetContinent` | Continent | Promote |  |
| `Scope.GetCountry` | Country | Promote |  |
| `Scope.GetCountryInteraction` | CountryInteraction | Promote |  |
| `Scope.GetCountryRank` | CountryRank | Promote |  |
| `Scope.GetCulture` | Culture | Promote |  |
| `Scope.GetCultureGroup` | CultureGroup | Promote |  |
| `Scope.GetDialect` | Dialect | Promote |  |
| `Scope.GetDisaster` | Disaster | Promote |  |
| `Scope.GetDisasterType` | DisasterType | Promote |  |
| `Scope.GetDisease` | Disease | Promote |  |
| `Scope.GetDiseaseOutbreak` | DiseaseOutbreak | Promote |  |
| `Scope.GetDynasty` | Dynasty | Promote |  |
| `Scope.GetEmploymentSystem` | EmploymentSystem | Promote |  |
| `Scope.GetEstate` | Estate | Promote |  |
| `Scope.GetEstatePrivilege` | EstatePrivilege | Promote |  |
| `Scope.GetEstateType` | EstateType | Promote |  |
| `Scope.GetEthnicity` | Ethnicity | Promote |  |
| `Scope.GetExploration` | Exploration | Promote |  |
| `Scope.GetFlagName` | CString | Function |  |
| `Scope.GetFormableCountry` | FormableCountry | Promote |  |
| `Scope.GetGenericAction` | GenericAction | Promote |  |
| `Scope.GetGod` | God | Promote |  |
| `Scope.GetGoods` | Goods | Promote |  |
| `Scope.GetGoodsDemand` | Goods | Promote |  |
| `Scope.GetGovernmentReform` | GovernmentReform | Promote |  |
| `Scope.GetGovernmentType` | GovernmentType | Promote |  |
| `Scope.GetHegemony` | Hegemony | Promote |  |
| `Scope.GetHeirSelection` | HeirSelection | Promote |  |
| `Scope.GetHolySite` | HolySite | Promote |  |
| `Scope.GetHolySiteDefinition` | HolySiteDefinition | Promote |  |
| `Scope.GetHolySiteType` | HolySiteType | Promote |  |
| `Scope.GetImperialCircle` | ImperialCircle | Promote |  |
| `Scope.GetIndustryPromotion` | IndustryPromotion | Promote |  |
| `Scope.GetIndustryType` | IndustryType | Promote |  |
| `Scope.GetInstitution` | Institution | Promote |  |
| `Scope.GetInternationalOrganization` | InternationalOrganization | Promote |  |
| `Scope.GetInternationalOrganizationType` | InternationalOrganizationType | Promote |  |
| `Scope.GetLandOwnershipRule` | LandOwnershipRule | Promote |  |
| `Scope.GetLanguage` | Language | Promote |  |
| `Scope.GetLaw` | Law | Promote |  |
| `Scope.GetLevySetup` | LevySetup | Promote |  |
| `Scope.GetList( Arg0 )` | [unregistered] | Function |  |
| `Scope.GetLoan` | Loan | Promote |  |
| `Scope.GetLocation` | Location | Promote |  |
| `Scope.GetLocationRank` | LocationRank | Promote |  |
| `Scope.GetMapKeys( Arg0 )` | [unregistered] | Function |  |
| `Scope.GetMarket` | Market | Promote |  |
| `Scope.GetMercenary` | Mercenary | Promote |  |
| `Scope.GetMissionDefinition` | MissionDefinition | Promote |  |
| `Scope.GetMissionTaskDefinition` | MissionTaskDefinition | Promote |  |
| `Scope.GetMovement` | Movement | Promote |  |
| `Scope.GetMovementDefinition` | MovementDefinition | Promote |  |
| `Scope.GetOmen` | Omen | Promote |  |
| `Scope.GetParliamentAgenda` | ParliamentAgenda | Promote |  |
| `Scope.GetParliamentIssue` | ParliamentIssue | Promote |  |
| `Scope.GetParliamentType` | ParliamentType | Promote |  |
| `Scope.GetPayment` | Payment | Promote |  |
| `Scope.GetPolicy` | Policy | Promote |  |
| `Scope.GetPop` | Pop | Promote |  |
| `Scope.GetPopType` | PopType | Promote |  |
| `Scope.GetPrice` | Price | Promote |  |
| `Scope.GetPrivateer` | Privateer | Promote |  |
| `Scope.GetProductionMethod` | ProductionMethod | Promote |  |
| `Scope.GetProvince` | Province | Promote |  |
| `Scope.GetProvinceDefinition` | ProvinceDefinition | Promote |  |
| `Scope.GetRebel` | Rebel | Promote |  |
| `Scope.GetRecruitmentMethod` | RecruitmentMethod | Promote |  |
| `Scope.GetRegencyType` | RegencyType | Promote |  |
| `Scope.GetRegion` | Region | Promote |  |
| `Scope.GetRelationType` | ScriptedRelationType | Promote |  |
| `Scope.GetReligion` | Religion | Promote |  |
| `Scope.GetReligionGroup` | ReligionGroup | Promote |  |
| `Scope.GetReligiousAspect` | ReligiousAspect | Promote |  |
| `Scope.GetReligiousFaction` | ReligiousFaction | Promote |  |
| `Scope.GetReligiousFigure` | ReligiousFigure | Promote |  |
| `Scope.GetReligiousFocus` | ReligiousFocus | Promote |  |
| `Scope.GetReligiousSchool` | ReligiousSchool | Promote |  |
| `Scope.GetResolution` | Resolution | Promote |  |
| `Scope.GetRoadType` | RoadType | Promote |  |
| `Scope.GetScriptableHintDefinition` | ScriptableHintDefinition | Promote |  |
| `Scope.GetScriptedGeography` | ScriptedGeography | Promote |  |
| `Scope.GetScriptedPeaceTreatyType` | ScriptedPeaceTreatyType | Promote |  |
| `Scope.GetScriptValueDesc( Arg0 )` | CString | Function |  |
| `Scope.GetSiege` | Siege | Promote |  |
| `Scope.GetSituation` | Situation | Promote |  |
| `Scope.GetSocietalValue` | SocietalValue | Promote |  |
| `Scope.GetSpecialStatus` | SpecialStatus | Promote |  |
| `Scope.GetSubContinent` | SubContinent | Promote |  |
| `Scope.GetSubjectMilitaryStance` | SubjectMilitaryStance | Promote |  |
| `Scope.GetSubjectType` | SubjectType | Promote |  |
| `Scope.GetSubUnit` | SubUnit | Promote |  |
| `Scope.GetSubUnitCategory` | SubUnitCategory | Promote |  |
| `Scope.GetTopography` | Topography | Promote |  |
| `Scope.GetTownRights` | TownRightsType | Promote |  |
| `Scope.GetTownRightsType` | TownRightsType | Promote |  |
| `Scope.GetTrade` | Trade | Promote |  |
| `Scope.GetTrait` | Trait | Promote |  |
| `Scope.GetUnit` | Unit | Promote |  |
| `Scope.GetUnitAbility` | UnitAbility | Promote |  |
| `Scope.GetUnitType` | SubUnitType | Promote |  |
| `Scope.GetValue` | CFixedPoint | Function |  |
| `Scope.GetValueWithDefault( Arg0 )` | CFixedPoint | Function |  |
| `Scope.GetVariable( Arg0 )` | Scope | Promote |  |
| `Scope.GetVariableFromVariableMap( Arg0, Arg1 )` | Scope | Promote |  |
| `Scope.GetVariableListTicks( Arg0, Arg1 )` | int32 | Function |  |
| `Scope.GetVariableMapTicks( Arg0, Arg1 )` | int32 | Function |  |
| `Scope.GetVariableTicks( Arg0 )` | int32 | Function |  |
| `Scope.GetVegetation` | Vegetation | Promote |  |
| `Scope.GetWar` | War | Promote |  |
| `Scope.GetWeatherSystem` | WeatherSystem | Promote |  |
| `Scope.GetWorkOfArt` | WorkOfArt | Promote |  |
| `Scope.GetWorkOfArtType` | WorkOfArtType | Promote |  |
| `Scope.God` | [unregistered] | Function | Jomini Script System |
| `Scope.God` | God | Promote | Jomini Script System |
| `Scope.Goods` | Goods | Promote | Jomini Script System |
| `Scope.Goods` | [unregistered] | Function | Jomini Script System |
| `Scope.GoodsDemand` | [unregistered] | Function | Jomini Script System |
| `Scope.GoodsDemand` | GoodsDemand | Promote | Jomini Script System |
| `Scope.GovernmentType` | GovernmentType | Promote | Jomini Script System |
| `Scope.GovernmentType` | [unregistered] | Function | Jomini Script System |
| `Scope.Hegemony` | [unregistered] | Function | Jomini Script System |
| `Scope.Hegemony` | Hegemony | Promote | Jomini Script System |
| `Scope.HeirSelection` | HeirSelection | Promote | Jomini Script System |
| `Scope.HeirSelection` | [unregistered] | Function | Jomini Script System |
| `Scope.HolySite` | HolySite | Promote | Jomini Script System |
| `Scope.HolySite` | [unregistered] | Function | Jomini Script System |
| `Scope.HolySiteDefinition` | [unregistered] | Function | Jomini Script System |
| `Scope.HolySiteDefinition` | HolySiteDefinition | Promote | Jomini Script System |
| `Scope.HolySiteType` | [unregistered] | Function | Jomini Script System |
| `Scope.HolySiteType` | HolySiteType | Promote | Jomini Script System |
| `Scope.ImperialCircle` | ImperialCircle | Promote | Jomini Script System |
| `Scope.ImperialCircle` | [unregistered] | Function | Jomini Script System |
| `Scope.IndustryPromotion` | IndustryPromotion | Promote | Jomini Script System |
| `Scope.IndustryPromotion` | [unregistered] | Function | Jomini Script System |
| `Scope.IndustryType` | IndustryType | Promote | Jomini Script System |
| `Scope.IndustryType` | [unregistered] | Function | Jomini Script System |
| `Scope.Institution` | Institution | Promote | Jomini Script System |
| `Scope.Institution` | [unregistered] | Function | Jomini Script System |
| `Scope.InternationalOrganization` | InternationalOrganization | Promote | Jomini Script System |
| `Scope.InternationalOrganization` | [unregistered] | Function | Jomini Script System |
| `Scope.InternationalOrganizationType` | InternationalOrganizationType | Promote | Jomini Script System |
| `Scope.InternationalOrganizationType` | [unregistered] | Function | Jomini Script System |
| `Scope.IsSet` | bool | Function |  |
| `Scope.LandOwnershipRule` | [unregistered] | Function | Jomini Script System |
| `Scope.LandOwnershipRule` | LandOwnershipRule | Promote | Jomini Script System |
| `Scope.Language` | [unregistered] | Function | Jomini Script System |
| `Scope.Language` | Language | Promote | Jomini Script System |
| `Scope.LanguageFamily` | [unregistered] | Function | Jomini Script System |
| `Scope.LanguageFamily` | LanguageFamily | Promote | Jomini Script System |
| `Scope.Law` | [unregistered] | Function | Jomini Script System |
| `Scope.Law` | Law | Promote | Jomini Script System |
| `Scope.LevySetup` | [unregistered] | Function | Jomini Script System |
| `Scope.LevySetup` | LevySetup | Promote | Jomini Script System |
| `Scope.Loan` | [unregistered] | Function | Jomini Script System |
| `Scope.Loan` | Loan | Promote | Jomini Script System |
| `Scope.Location` | Location | Promote | Jomini Script System |
| `Scope.Location` | [unregistered] | Function | Jomini Script System |
| `Scope.LocationRank` | [unregistered] | Function | Jomini Script System |
| `Scope.LocationRank` | LocationRank | Promote | Jomini Script System |
| `Scope.Market` | [unregistered] | Function | Jomini Script System |
| `Scope.Market` | Market | Promote | Jomini Script System |
| `Scope.Mercenary` | Mercenary | Promote | Jomini Script System |
| `Scope.Mercenary` | [unregistered] | Function | Jomini Script System |
| `Scope.MissionDefinition` | MissionDefinition | Promote | Jomini Script System |
| `Scope.MissionDefinition` | [unregistered] | Function | Jomini Script System |
| `Scope.MissionTaskDefinition` | [unregistered] | Function | Jomini Script System |
| `Scope.MissionTaskDefinition` | MissionTaskDefinition | Promote | Jomini Script System |
| `Scope.Movement` | Movement | Promote | Jomini Script System |
| `Scope.Movement` | [unregistered] | Function | Jomini Script System |
| `Scope.MovementDefinition` | [unregistered] | Function | Jomini Script System |
| `Scope.MovementDefinition` | MovementDefinition | Promote | Jomini Script System |
| `Scope.Omen` | Omen | Promote | Jomini Script System |
| `Scope.Omen` | [unregistered] | Function | Jomini Script System |
| `Scope.ParliamentAgenda` | [unregistered] | Function | Jomini Script System |
| `Scope.ParliamentAgenda` | ParliamentAgenda | Promote | Jomini Script System |
| `Scope.ParliamentIssue` | ParliamentIssue | Promote | Jomini Script System |
| `Scope.ParliamentIssue` | [unregistered] | Function | Jomini Script System |
| `Scope.ParliamenType` | [unregistered] | Function | Jomini Script System |
| `Scope.ParliamenType` | ParliamentType | Promote | Jomini Script System |
| `Scope.Payment` | Payment | Promote | Jomini Script System |
| `Scope.Payment` | [unregistered] | Function | Jomini Script System |
| `Scope.Policy` | [unregistered] | Function | Jomini Script System |
| `Scope.Policy` | Policy | Promote | Jomini Script System |
| `Scope.Pop` | Pop | Promote | Jomini Script System |
| `Scope.Pop` | [unregistered] | Function | Jomini Script System |
| `Scope.PopType` | PopType | Promote | Jomini Script System |
| `Scope.PopType` | [unregistered] | Function | Jomini Script System |
| `Scope.Price` | [unregistered] | Function | Jomini Script System |
| `Scope.Price` | Price | Promote | Jomini Script System |
| `Scope.Privateer` | [unregistered] | Function | Jomini Script System |
| `Scope.Privateer` | Privateer | Promote | Jomini Script System |
| `Scope.ProductionMethod` | ProductionMethod | Promote | Jomini Script System |
| `Scope.ProductionMethod` | [unregistered] | Function | Jomini Script System |
| `Scope.Province` | [unregistered] | Function | Jomini Script System |
| `Scope.Province` | Province | Promote | Jomini Script System |
| `Scope.ProvinceDefinition` | [unregistered] | Function | Jomini Script System |
| `Scope.ProvinceDefinition` | ProvinceDefinition | Promote | Jomini Script System |
| `Scope.Rebel` | [unregistered] | Function | Jomini Script System |
| `Scope.Rebel` | Rebel | Promote | Jomini Script System |
| `Scope.RecruitmentMethod` | RecruitmentMethod | Promote | Jomini Script System |
| `Scope.RecruitmentMethod` | [unregistered] | Function | Jomini Script System |
| `Scope.RegencyType` | [unregistered] | Function | Jomini Script System |
| `Scope.RegencyType` | RegencyType | Promote | Jomini Script System |
| `Scope.Region` | [unregistered] | Function | Jomini Script System |
| `Scope.Region` | Region | Promote | Jomini Script System |
| `Scope.RelationType` | [unregistered] | Function | Jomini Script System |
| `Scope.RelationType` | ScriptedRelationType | Promote | Jomini Script System |
| `Scope.Religion` | [unregistered] | Function | Jomini Script System |
| `Scope.Religion` | Religion | Promote | Jomini Script System |
| `Scope.ReligionGroup` | ReligionGroup | Promote | Jomini Script System |
| `Scope.ReligionGroup` | [unregistered] | Function | Jomini Script System |
| `Scope.ReligiousAspect` | [unregistered] | Function | Jomini Script System |
| `Scope.ReligiousAspect` | ReligiousAspect | Promote | Jomini Script System |
| `Scope.ReligiousFigure` | ReligiousFigure | Promote | Jomini Script System |
| `Scope.ReligiousFigure` | [unregistered] | Function | Jomini Script System |
| `Scope.ReligiousFocus` | [unregistered] | Function | Jomini Script System |
| `Scope.ReligiousFocus` | ReligiousFocus | Promote | Jomini Script System |
| `Scope.ReligiousOrder` | ReligiousOrder | Promote | Jomini Script System |
| `Scope.ReligiousOrder` | [unregistered] | Function | Jomini Script System |
| `Scope.ReligiousOrderType` | ReligiousOrderType | Promote | Jomini Script System |
| `Scope.ReligiousOrderType` | [unregistered] | Function | Jomini Script System |
| `Scope.ReligiousSchool` | [unregistered] | Function | Jomini Script System |
| `Scope.ReligiousSchool` | ReligiousSchool | Promote | Jomini Script System |
| `Scope.Resolution` | [unregistered] | Function | Jomini Script System |
| `Scope.Resolution` | Resolution | Promote | Jomini Script System |
| `Scope.RoadType` | [unregistered] | Function | Jomini Script System |
| `Scope.RoadType` | RoadType | Promote | Jomini Script System |
| `Scope.ScriptableHintDefinition` | [unregistered] | Function | Jomini Script System |
| `Scope.ScriptableHintDefinition` | ScriptableHintDefinition | Promote | Jomini Script System |
| `Scope.ScriptedGeography` | ScriptedGeography | Promote | Jomini Script System |
| `Scope.ScriptedGeography` | [unregistered] | Function | Jomini Script System |
| `Scope.ScriptedPeaceTreatyType` | [unregistered] | Function | Jomini Script System |
| `Scope.ScriptedPeaceTreatyType` | ScriptedPeaceTreatyType | Promote | Jomini Script System |
| `Scope.ScriptValue( Arg0 )` | CFixedPoint | Function |  |
| `Scope.Self` | [unregistered] | Function |  |
| `Scope.Siege` | [unregistered] | Function | Jomini Script System |
| `Scope.Siege` | Siege | Promote | Jomini Script System |
| `Scope.Situation` | [unregistered] | Function | Jomini Script System |
| `Scope.Situation` | Situation | Promote | Jomini Script System |
| `Scope.SocietalValue` | SocietalValue | Promote | Jomini Script System |
| `Scope.SocietalValue` | [unregistered] | Function | Jomini Script System |
| `Scope.SpecialStatus` | [unregistered] | Function | Jomini Script System |
| `Scope.SpecialStatus` | SpecialStatus | Promote | Jomini Script System |
| `Scope.SubContinent` | [unregistered] | Function | Jomini Script System |
| `Scope.SubContinent` | SubContinent | Promote | Jomini Script System |
| `Scope.SubjectMilitaryStance` | SubjectMilitaryStance | Promote | Jomini Script System |
| `Scope.SubjectMilitaryStance` | [unregistered] | Function | Jomini Script System |
| `Scope.SubjectType` | [unregistered] | Function | Jomini Script System |
| `Scope.SubjectType` | SubjectType | Promote | Jomini Script System |
| `Scope.SubUnit` | [unregistered] | Function | Jomini Script System |
| `Scope.SubUnit` | SubUnit | Promote | Jomini Script System |
| `Scope.SubUnitCategory` | [unregistered] | Function | Jomini Script System |
| `Scope.SubUnitCategory` | SubUnitCategory | Promote | Jomini Script System |
| `Scope.Topography` | Topography | Promote | Jomini Script System |
| `Scope.Topography` | [unregistered] | Function | Jomini Script System |
| `Scope.TownRights` | [unregistered] | Function | Jomini Script System |
| `Scope.TownRights` | TownRights | Promote | Jomini Script System |
| `Scope.TownRightsType` | [unregistered] | Function | Jomini Script System |
| `Scope.TownRightsType` | TownRightsType | Promote | Jomini Script System |
| `Scope.Trade` | Trade | Promote | Jomini Script System |
| `Scope.Trade` | [unregistered] | Function | Jomini Script System |
| `Scope.Trait` | [unregistered] | Function | Jomini Script System |
| `Scope.Trait` | Trait | Promote | Jomini Script System |
| `Scope.Unit` | Unit | Promote | Jomini Script System |
| `Scope.Unit` | [unregistered] | Function | Jomini Script System |
| `Scope.UnitAbility` | [unregistered] | Function | Jomini Script System |
| `Scope.UnitAbility` | UnitAbility | Promote | Jomini Script System |
| `Scope.UnitFormationPreference` | UnitFormationPreference | Promote | Jomini Script System |
| `Scope.UnitFormationPreference` | [unregistered] | Function | Jomini Script System |
| `Scope.UnitType` | SubUnitType | Promote | Jomini Script System |
| `Scope.UnitType` | [unregistered] | Function | Jomini Script System |
| `Scope.Var( Arg0 )` | Scope | Promote |  |
| `Scope.VariableMapContainsKey( Arg0, Arg1 )` | bool | Function |  |
| `Scope.VariableMapContainsValue( Arg0, Arg1 )` | bool | Function |  |
| `Scope.VariableMapExists( Arg0 )` | [unregistered] | Function |  |
| `Scope.VarRemaining( Arg0 )` | int32 | Function |  |
| `Scope.Vegetation` | [unregistered] | Function | Jomini Script System |
| `Scope.Vegetation` | Vegetation | Promote | Jomini Script System |
| `Scope.War` | War | Promote | Jomini Script System |
| `Scope.War` | [unregistered] | Function | Jomini Script System |
| `Scope.WeatherSystem` | WeatherSystem | Promote | Jomini Script System |
| `Scope.WeatherSystem` | [unregistered] | Function | Jomini Script System |
| `Scope.WorkOfArt` | [unregistered] | Function | Jomini Script System |
| `Scope.WorkOfArt` | WorkOfArt | Promote | Jomini Script System |
| `Scope.WorkOfArtType` | WorkOfArtType | Promote | Jomini Script System |
| `Scope.WorkOfArtType` | [unregistered] | Function | Jomini Script System |
| `ScriptableHintDefinition` |  | Type |  |
| `ScriptableHintDefinition.MakeScope` | Scope | Function | Jomini Script System |
| `ScriptedGeography` |  | Type |  |
| `ScriptedGeography.MakeScope` | Scope | Function | Jomini Script System |
| `ScriptedGui` |  | Type |  |
| `ScriptedGui.AccessSelf` | [unregistered] | Function |  |
| `ScriptedGui.BuildTooltip( Arg0 )` | CString | Function |  |
| `ScriptedGui.Execute( Arg0 )` | void | Function |  |
| `ScriptedGui.ExecuteTooltip( Arg0 )` | CString | Function |  |
| `ScriptedGui.IsShown( Arg0 )` | bool | Function |  |
| `ScriptedGui.IsShownTooltip( Arg0 )` | CString | Function |  |
| `ScriptedGui.IsValid( Arg0 )` | bool | Function |  |
| `ScriptedGui.IsValidTooltip( Arg0 )` | CString | Function |  |
| `ScriptedGui.Self` | [unregistered] | Function |  |
| `ScriptedPeaceTreatyType` |  | Type |  |
| `ScriptedPeaceTreatyType.MakeScope` | Scope | Function | Jomini Script System |
| `ScriptedRelationType` |  | Type |  |
| `ScriptedRelationType.MakeScope` | Scope | Function | Jomini Script System |
| `Siege` |  | Type |  |
| `Siege.MakeScope` | Scope | Function | Jomini Script System |
| `Situation` |  | Type |  |
| `Situation.MakeScope` | Scope | Function | Jomini Script System |
| `SocietalValue` |  | Type |  |
| `SocietalValue.MakeScope` | Scope | Function | Jomini Script System |
| `SpecialStatus` |  | Type |  |
| `SpecialStatus.MakeScope` | Scope | Function | Jomini Script System |
| `SubContinent` |  | Type |  |
| `SubContinent.MakeScope` | Scope | Function | Jomini Script System |
| `SubjectMilitaryStance` |  | Type |  |
| `SubjectMilitaryStance.MakeScope` | Scope | Function | Jomini Script System |
| `SubjectType` |  | Type |  |
| `SubjectType.MakeScope` | Scope | Function | Jomini Script System |
| `SubUnit` |  | Type |  |
| `SubUnit.MakeScope` | Scope | Function | Jomini Script System |
| `SubUnitCategory` |  | Type |  |
| `SubUnitCategory.MakeScope` | Scope | Function | Jomini Script System |
| `SubUnitType` |  | Type |  |
| `SubUnitType.MakeScope` | Scope | Function | Jomini Script System |
| `THIS` | Scope | Global promote | Script Scope |
| `Topography` |  | Type |  |
| `Topography.MakeScope` | Scope | Function | Jomini Script System |
| `TopScope` |  | Type |  |
| `TopScope.AccessSelf` | [unregistered] | Function |  |
| `TopScope.AddList( Arg0, Arg1 )` | TopScope | Promote |  |
| `TopScope.AddScope( Arg0, Arg1 )` | TopScope | Promote |  |
| `TopScope.End` | TopScope | Function |  |
| `TopScope.GetFlagName( Arg0 )` | CString | Function |  |
| `TopScope.GetLocalVariable( Arg0 )` | Scope | Promote |  |
| `TopScope.GetLocalVariable( Arg0 )` | [unregistered] | Function |  |
| `TopScope.GetRootScope` | [unregistered] | Function |  |
| `TopScope.GetRootScope` | Scope | Promote |  |
| `TopScope.GetScriptValueDesc( Arg0 )` | CString | Function |  |
| `TopScope.GetValue( Arg0 )` | CFixedPoint | Function |  |
| `TopScope.sActiveResolution( Arg0 )` | ActiveResolution | Promote |  |
| `TopScope.sActiveResolution( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sAdvanceType( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sAdvanceType( Arg0 )` | AdvanceDefinition | Promote |  |
| `TopScope.sAge( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sAge( Arg0 )` | Age | Promote |  |
| `TopScope.sAmbition( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sAmbition( Arg0 )` | Ambition | Promote |  |
| `TopScope.sAmbitionDefinition( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sAmbitionDefinition( Arg0 )` | AmbitionDefinition | Promote |  |
| `TopScope.sArea( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sArea( Arg0 )` | Area | Promote |  |
| `TopScope.sArtist( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sArtist( Arg0 )` | Artist | Promote |  |
| `TopScope.sAudioCulture( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sAudioCulture( Arg0 )` | AudioCultureType | Promote |  |
| `TopScope.sAvatar( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sAvatar( Arg0 )` | Avatar | Promote |  |
| `TopScope.sBuildingType( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sBuildingType( Arg0 )` | BuildingType | Promote |  |
| `TopScope.sBureaucracy( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sBureaucracy( Arg0 )` | Bureaucracy | Promote |  |
| `TopScope.sBureaucracyType( Arg0 )` | BureaucracyType | Promote |  |
| `TopScope.sBureaucracyType( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sCabinet( Arg0 )` | Cabinet | Promote |  |
| `TopScope.sCabinet( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sCabinetAction( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sCabinetAction( Arg0 )` | CabinetAction | Promote |  |
| `TopScope.sCasusBelli( Arg0 )` | CasusBelli | Promote |  |
| `TopScope.sCasusBelli( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sCharacter( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sCharacter( Arg0 )` | Character | Promote |  |
| `TopScope.sCharacterInteraction( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sCharacterInteraction( Arg0 )` | CharacterInteraction | Promote |  |
| `TopScope.sChildEducation( Arg0 )` | ChildEducation | Promote |  |
| `TopScope.sChildEducation( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sChivalricOrder( Arg0 )` | ChivalricOrder | Promote |  |
| `TopScope.sChivalricOrder( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sClimate( Arg0 )` | Climate | Promote |  |
| `TopScope.sClimate( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sColonialCharter( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sColonialCharter( Arg0 )` | ColonialCharter | Promote |  |
| `TopScope.sCombat( Arg0 )` | Combat | Promote |  |
| `TopScope.sCombat( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sCombatSide( Arg0 )` | CombatSide | Promote |  |
| `TopScope.sCombatSide( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sContinent( Arg0 )` | Continent | Promote |  |
| `TopScope.sContinent( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sCountry( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sCountry( Arg0 )` | Country | Promote |  |
| `TopScope.sCountryInteraction( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sCountryInteraction( Arg0 )` | CountryInteraction | Promote |  |
| `TopScope.sCountryRank( Arg0 )` | CountryRank | Promote |  |
| `TopScope.sCountryRank( Arg0 )` | [unregistered] | Function |  |
| `TopScope.ScriptValue( Arg0 )` | CFixedPoint | Function |  |
| `TopScope.sCulture( Arg0 )` | Culture | Promote |  |
| `TopScope.sCulture( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sDialect( Arg0 )` | Dialect | Promote |  |
| `TopScope.sDialect( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sDisaster( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sDisaster( Arg0 )` | Disaster | Promote |  |
| `TopScope.sDisasterType( Arg0 )` | DisasterType | Promote |  |
| `TopScope.sDisasterType( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sDisease( Arg0 )` | Disease | Promote |  |
| `TopScope.sDisease( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sDiseaseOutbreak( Arg0 )` | DiseaseOutbreak | Promote |  |
| `TopScope.sDiseaseOutbreak( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sDynasty( Arg0 )` | Dynasty | Promote |  |
| `TopScope.sDynasty( Arg0 )` | [unregistered] | Function |  |
| `TopScope.Self` | [unregistered] | Function |  |
| `TopScope.sEmploymentSystem( Arg0 )` | EmploymentSystem | Promote |  |
| `TopScope.sEmploymentSystem( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sEstate( Arg0 )` | Estate | Promote |  |
| `TopScope.sEstate( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sEstatePrivilege( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sEstatePrivilege( Arg0 )` | EstatePrivilege | Promote |  |
| `TopScope.sEstateType( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sEstateType( Arg0 )` | EstateType | Promote |  |
| `TopScope.sEthnicity( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sEthnicity( Arg0 )` | Ethnicity | Promote |  |
| `TopScope.SetRoot( Arg0 )` | TopScope | Promote |  |
| `TopScope.sExpedition( Arg0 )` | Expedition | Promote |  |
| `TopScope.sExpedition( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sExpeditionType( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sExpeditionType( Arg0 )` | ExpeditionType | Promote |  |
| `TopScope.sExploration( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sExploration( Arg0 )` | Exploration | Promote |  |
| `TopScope.sFormableCountry( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sFormableCountry( Arg0 )` | FormableCountry | Promote |  |
| `TopScope.sGenericAction( Arg0 )` | GenericAction | Promote |  |
| `TopScope.sGenericAction( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sGod( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sGod( Arg0 )` | God | Promote |  |
| `TopScope.sGoods( Arg0 )` | Goods | Promote |  |
| `TopScope.sGoods( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sGoodsDemand( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sGoodsDemand( Arg0 )` | GoodsDemand | Promote |  |
| `TopScope.sGovernmentReform( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sGovernmentReform( Arg0 )` | GovernmentReform | Promote |  |
| `TopScope.sGovernmentType( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sGovernmentType( Arg0 )` | GovernmentType | Promote |  |
| `TopScope.sGraphicalCulture( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sGraphicalCulture( Arg0 )` | GraphicalCultureType | Promote |  |
| `TopScope.sHegemony( Arg0 )` | Hegemony | Promote |  |
| `TopScope.sHegemony( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sHeirSelection( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sHeirSelection( Arg0 )` | HeirSelection | Promote |  |
| `TopScope.sHolySite( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sHolySite( Arg0 )` | HolySite | Promote |  |
| `TopScope.sHolySiteDefinition( Arg0 )` | HolySiteDefinition | Promote |  |
| `TopScope.sHolySiteDefinition( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sHolySiteType( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sHolySiteType( Arg0 )` | HolySiteType | Promote |  |
| `TopScope.sImperialCircle( Arg0 )` | ImperialCircle | Promote |  |
| `TopScope.sImperialCircle( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sIndustryPromotion( Arg0 )` | IndustryPromotion | Promote |  |
| `TopScope.sIndustryPromotion( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sIndustryType( Arg0 )` | IndustryType | Promote |  |
| `TopScope.sIndustryType( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sInstitution( Arg0 )` | Institution | Promote |  |
| `TopScope.sInstitution( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sInternationalOrganization( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sInternationalOrganization( Arg0 )` | InternationalOrganization | Promote |  |
| `TopScope.sInternationalOrganizationType( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sInternationalOrganizationType( Arg0 )` | InternationalOrganizationType | Promote |  |
| `TopScope.sLandOwnershipRule( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sLandOwnershipRule( Arg0 )` | LandOwnershipRule | Promote |  |
| `TopScope.sLanguage( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sLanguage( Arg0 )` | Language | Promote |  |
| `TopScope.sLanguageFamily( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sLanguageFamily( Arg0 )` | LanguageFamily | Promote |  |
| `TopScope.sLaw( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sLaw( Arg0 )` | Law | Promote |  |
| `TopScope.sLevySetup( Arg0 )` | LevySetup | Promote |  |
| `TopScope.sLevySetup( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sLoan( Arg0 )` | Loan | Promote |  |
| `TopScope.sLoan( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sLocation( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sLocation( Arg0 )` | Location | Promote |  |
| `TopScope.sLocationRank( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sLocationRank( Arg0 )` | LocationRank | Promote |  |
| `TopScope.sMarket( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sMarket( Arg0 )` | Market | Promote |  |
| `TopScope.sMercenary( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sMercenary( Arg0 )` | Mercenary | Promote |  |
| `TopScope.sMissionDefinition( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sMissionDefinition( Arg0 )` | MissionDefinition | Promote |  |
| `TopScope.sMissionTaskDefinition( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sMissionTaskDefinition( Arg0 )` | MissionTaskDefinition | Promote |  |
| `TopScope.sMovement( Arg0 )` | Movement | Promote |  |
| `TopScope.sMovement( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sMovementDefinition( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sMovementDefinition( Arg0 )` | MovementDefinition | Promote |  |
| `TopScope.sOmen( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sOmen( Arg0 )` | Omen | Promote |  |
| `TopScope.sParliamentAgenda( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sParliamentAgenda( Arg0 )` | ParliamentAgenda | Promote |  |
| `TopScope.sParliamentIssue( Arg0 )` | ParliamentIssue | Promote |  |
| `TopScope.sParliamentIssue( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sParliamentType( Arg0 )` | ParliamentType | Promote |  |
| `TopScope.sParliamentType( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sPayment( Arg0 )` | Payment | Promote |  |
| `TopScope.sPayment( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sPolicy( Arg0 )` | Policy | Promote |  |
| `TopScope.sPolicy( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sPop( Arg0 )` | Pop | Promote |  |
| `TopScope.sPop( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sPopType( Arg0 )` | PopType | Promote |  |
| `TopScope.sPopType( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sPrice( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sPrice( Arg0 )` | Price | Promote |  |
| `TopScope.sPrivateer( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sPrivateer( Arg0 )` | Privateer | Promote |  |
| `TopScope.sProductionMethod( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sProductionMethod( Arg0 )` | ProductionMethod | Promote |  |
| `TopScope.sProvince( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sProvince( Arg0 )` | Province | Promote |  |
| `TopScope.sProvinceDefinition( Arg0 )` | ProvinceDefinition | Promote |  |
| `TopScope.sProvinceDefinition( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sRebel( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sRebel( Arg0 )` | Rebel | Promote |  |
| `TopScope.sRecruitmentMethod( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sRecruitmentMethod( Arg0 )` | RecruitmentMethod | Promote |  |
| `TopScope.sRegencyType( Arg0 )` | RegencyType | Promote |  |
| `TopScope.sRegencyType( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sRegion( Arg0 )` | Region | Promote |  |
| `TopScope.sRegion( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sRelationType( Arg0 )` | ScriptedRelationType | Promote |  |
| `TopScope.sRelationType( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sReligion( Arg0 )` | Religion | Promote |  |
| `TopScope.sReligion( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sReligionGroup( Arg0 )` | ReligionGroup | Promote |  |
| `TopScope.sReligionGroup( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sReligiousAspect( Arg0 )` | ReligiousAspect | Promote |  |
| `TopScope.sReligiousAspect( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sReligiousFaction( Arg0 )` | ReligiousFaction | Promote |  |
| `TopScope.sReligiousFaction( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sReligiousFigure( Arg0 )` | ReligiousFigure | Promote |  |
| `TopScope.sReligiousFigure( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sReligiousFocus( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sReligiousFocus( Arg0 )` | ReligiousFocus | Promote |  |
| `TopScope.sReligiousOrder( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sReligiousOrder( Arg0 )` | ReligiousOrder | Promote |  |
| `TopScope.sReligiousOrderType( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sReligiousOrderType( Arg0 )` | ReligiousOrderType | Promote |  |
| `TopScope.sReligiousSchool( Arg0 )` | ReligiousSchool | Promote |  |
| `TopScope.sReligiousSchool( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sResolution( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sResolution( Arg0 )` | Resolution | Promote |  |
| `TopScope.sRoadType( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sRoadType( Arg0 )` | RoadType | Promote |  |
| `TopScope.sScriptableHintDefinition( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sScriptableHintDefinition( Arg0 )` | ScriptableHintDefinition | Promote |  |
| `TopScope.sScriptedGeography( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sScriptedGeography( Arg0 )` | ScriptedGeography | Promote |  |
| `TopScope.sScriptedPeaceTreatyType( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sScriptedPeaceTreatyType( Arg0 )` | ScriptedPeaceTreatyType | Promote |  |
| `TopScope.sSiege( Arg0 )` | Siege | Promote |  |
| `TopScope.sSiege( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sSituation( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sSituation( Arg0 )` | Situation | Promote |  |
| `TopScope.sSocietalValue( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sSocietalValue( Arg0 )` | SocietalValue | Promote |  |
| `TopScope.sSpecialStatus( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sSpecialStatus( Arg0 )` | SpecialStatus | Promote |  |
| `TopScope.sSubContinent( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sSubContinent( Arg0 )` | SubContinent | Promote |  |
| `TopScope.sSubjectMilitaryStance( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sSubjectMilitaryStance( Arg0 )` | SubjectMilitaryStance | Promote |  |
| `TopScope.sSubjectType( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sSubjectType( Arg0 )` | SubjectType | Promote |  |
| `TopScope.sSubUnit( Arg0 )` | SubUnit | Promote |  |
| `TopScope.sSubUnit( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sSubUnitCategory( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sSubUnitCategory( Arg0 )` | SubUnitCategory | Promote |  |
| `TopScope.sTopography( Arg0 )` | Topography | Promote |  |
| `TopScope.sTopography( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sTownRights( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sTownRights( Arg0 )` | TownRights | Promote |  |
| `TopScope.sTownRightsType( Arg0 )` | TownRightsType | Promote |  |
| `TopScope.sTownRightsType( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sTrade( Arg0 )` | Trade | Promote |  |
| `TopScope.sTrade( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sTrait( Arg0 )` | Trait | Promote |  |
| `TopScope.sTrait( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sUnit( Arg0 )` | Unit | Promote |  |
| `TopScope.sUnit( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sUnitAbility( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sUnitAbility( Arg0 )` | UnitAbility | Promote |  |
| `TopScope.sUnitFormationPreference( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sUnitFormationPreference( Arg0 )` | UnitFormationPreference | Promote |  |
| `TopScope.sUnitTemplate( Arg0 )` | UnitTemplate | Promote |  |
| `TopScope.sUnitTemplate( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sUnitType( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sUnitType( Arg0 )` | SubUnitType | Promote |  |
| `TopScope.sVegetation( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sVegetation( Arg0 )` | Vegetation | Promote |  |
| `TopScope.sWar( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sWar( Arg0 )` | War | Promote |  |
| `TopScope.sWeatherSystem( Arg0 )` | WeatherSystem | Promote |  |
| `TopScope.sWeatherSystem( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sWorkOfArt( Arg0 )` | WorkOfArt | Promote |  |
| `TopScope.sWorkOfArt( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sWorkOfArtType( Arg0 )` | [unregistered] | Function |  |
| `TopScope.sWorkOfArtType( Arg0 )` | WorkOfArtType | Promote |  |
| `TownRights` |  | Type |  |
| `TownRights.MakeScope` | Scope | Function | Jomini Script System |
| `TownRightsType` |  | Type |  |
| `TownRightsType.MakeScope` | Scope | Function | Jomini Script System |
| `Trade` |  | Type |  |
| `Trade.MakeScope` | Scope | Function | Jomini Script System |
| `Trait` |  | Type |  |
| `Trait.MakeScope` | Scope | Function | Jomini Script System |
| `Unit` |  | Type |  |
| `Unit.MakeScope` | Scope | Function | Jomini Script System |
| `UnitAbility` |  | Type |  |
| `UnitAbility.MakeScope` | Scope | Function | Jomini Script System |
| `UnitFormationPreference` |  | Type |  |
| `UnitFormationPreference.MakeScope` | Scope | Function | Jomini Script System |
| `Vegetation` |  | Type |  |
| `Vegetation.MakeScope` | Scope | Function | Jomini Script System |
| `War` |  | Type |  |
| `War.MakeScope` | Scope | Function | Jomini Script System |
| `WeatherSystem` |  | Type |  |
| `WeatherSystem.MakeScope` | Scope | Function | Jomini Script System |
| `WorkOfArt` |  | Type |  |
| `WorkOfArt.MakeScope` | Scope | Function | Jomini Script System |
| `WorkOfArtType` |  | Type |  |
| `WorkOfArtType.MakeScope` | Scope | Function | Jomini Script System |

## data_types_common.txt（345 条）

| 名称 | 返回类型 | 定义类型 | 说明 |
|---|---|---|---|
| `Abs_CFixedPoint( Arg0 )` | CFixedPoint | Global function |  |
| `Abs_float( Arg0 )` | float | Global function |  |
| `Abs_int32( Arg0 )` | int32 | Global function |  |
| `Abs_int64( Arg0 )` | int64 | Global function |  |
| `ActiveResolutionContainer` |  | Type | Base class for resolution containers |
| `ActiveResolutionContainer.AccessSelf` | [unregistered] | Function |  |
| `ActiveResolutionContainer.GetActiveResolution( Arg0 )` | [unregistered] | Function |  |
| `ActiveResolutionContainer.GetActiveResolution( Arg0 )` | ActiveResolution | Promote |  |
| `ActiveResolutionContainer.GetActiveResolutionFromKey( Arg0 )` | ActiveResolution | Promote |  |
| `ActiveResolutionContainer.GetActiveResolutionFromKey( Arg0 )` | [unregistered] | Function |  |
| `ActiveResolutionContainer.HasActiveResolution( Arg0 )` | bool | Function |  |
| `ActiveResolutionContainer.HasActiveResolutionFromKey( Arg0 )` | bool | Function |  |
| `ActiveResolutionContainer.ResolutionIsActive( Arg0 )` | bool | Function |  |
| `ActiveResolutionContainer.Self` | [unregistered] | Function |  |
| `Add_CFixedPoint( Arg0, Arg1 )` | CFixedPoint | Global function |  |
| `Add_CVector2f( Arg0, Arg1 )` | CVector2f | Global function |  |
| `Add_float( Arg0, Arg1 )` | float | Global function |  |
| `Add_int32( Arg0, Arg1 )` | int32 | Global function |  |
| `Add_int64( Arg0, Arg1 )` | int64 | Global function |  |
| `Add_uint32( Arg0, Arg1 )` | uint32 | Global function |  |
| `Add_uint64( Arg0, Arg1 )` | uint64 | Global function |  |
| `AddLocalizationIf( Arg0, Arg1 )` | CString | Global function |  |
| `AddTextIf( Arg0, Arg1 )` | CString | Global function |  |
| `And( Arg0, Arg1 )` | bool | Global function | If both arguments are true |
| `And3( Arg0, Arg1, Arg2 )` | bool | Global function | If all 3 arguments are true |
| `And4( Arg0, Arg1, Arg2, Arg3 )` | bool | Global function | If all 4 arguments are true |
| `And5( Arg0, Arg1, Arg2, Arg3, Arg4 )` | bool | Global function | If all 5 arguments are true |
| `And6( Arg0, Arg1, Arg2, Arg3, Arg4, Arg5 )` | bool | Global function | If all 6 arguments are true |
| `And7( Arg0, Arg1, Arg2, Arg3, Arg4, Arg5, Arg6 )` | bool | Global function | If all 7 arguments are true |
| `And8( Arg0, Arg1, Arg2, Arg3, Arg4, Arg5, Arg6, Arg7 )` | bool | Global function | If all 8 arguments are true |
| `AndEvalAll( Arg0, Arg1 )` | bool | Global function | Evaluates all arguments and then checks if both arguments are true. Using 'And' is preferred. |
| `Application` |  | Type |  |
| `Application` | Application | Global promote |  |
| `Application.AccessSelf` | [unregistered] | Function |  |
| `Application.Quit` | void | Function |  |
| `Application.Self` | [unregistered] | Function |  |
| `Between_CFixedPoint( Arg0, Arg1, Arg2 )` | bool | Global function |  |
| `Between_float( Arg0, Arg1, Arg2 )` | bool | Global function |  |
| `Between_int32( Arg0, Arg1, Arg2 )` | bool | Global function |  |
| `Between_int64( Arg0, Arg1, Arg2 )` | bool | Global function |  |
| `Between_uint32( Arg0, Arg1, Arg2 )` | bool | Global function |  |
| `Between_uint64( Arg0, Arg1, Arg2 )` | bool | Global function |  |
| `BetweenInclusive_CFixedPoint( Arg0, Arg1, Arg2 )` | bool | Global function |  |
| `BetweenInclusive_float( Arg0, Arg1, Arg2 )` | bool | Global function |  |
| `BetweenInclusive_int32( Arg0, Arg1, Arg2 )` | bool | Global function |  |
| `BetweenInclusive_int64( Arg0, Arg1, Arg2 )` | bool | Global function |  |
| `BetweenInclusive_uint32( Arg0, Arg1, Arg2 )` | bool | Global function |  |
| `BetweenInclusive_uint64( Arg0, Arg1, Arg2 )` | bool | Global function |  |
| `BetweenInclusiveOfMax_CFixedPoint( Arg0, Arg1, Arg2 )` | bool | Global function |  |
| `BetweenInclusiveOfMax_float( Arg0, Arg1, Arg2 )` | bool | Global function |  |
| `BetweenInclusiveOfMax_int32( Arg0, Arg1, Arg2 )` | bool | Global function |  |
| `BetweenInclusiveOfMax_int64( Arg0, Arg1, Arg2 )` | bool | Global function |  |
| `BetweenInclusiveOfMax_uint32( Arg0, Arg1, Arg2 )` | bool | Global function |  |
| `BetweenInclusiveOfMax_uint64( Arg0, Arg1, Arg2 )` | bool | Global function |  |
| `BetweenInclusiveOfMin_CFixedPoint( Arg0, Arg1, Arg2 )` | bool | Global function |  |
| `BetweenInclusiveOfMin_float( Arg0, Arg1, Arg2 )` | bool | Global function |  |
| `BetweenInclusiveOfMin_int32( Arg0, Arg1, Arg2 )` | bool | Global function |  |
| `BetweenInclusiveOfMin_int64( Arg0, Arg1, Arg2 )` | bool | Global function |  |
| `BetweenInclusiveOfMin_uint32( Arg0, Arg1, Arg2 )` | bool | Global function |  |
| `BetweenInclusiveOfMin_uint64( Arg0, Arg1, Arg2 )` | bool | Global function |  |
| `bool` |  | Type |  |
| `bool.AccessSelf` | [unregistered] | Function |  |
| `bool.Self` | [unregistered] | Function |  |
| `BoolTo1And2( Arg0 )` | int32 | Global function |  |
| `BoolTo2And1( Arg0 )` | int32 | Global function |  |
| `CFixedPoint` |  | Type | Number stored with 6 digits of precision. |
| `CFixedPoint.AccessSelf` | [unregistered] | Function |  |
| `CFixedPoint.GetFixedPoint` | [unregistered] | Function |  |
| `CFixedPoint.Self` | [unregistered] | Function |  |
| `Concatenate( Arg0, Arg1 )` | CString | Global function |  |
| `ConcatIfNeitherEmpty( Arg0, Arg1 )` | CString | Global function |  |
| `CPdxFloatRect` |  | Type | Rectangle within an 2D float space. |
| `CPdxFloatRect.AccessSelf` | [unregistered] | Function |  |
| `CPdxFloatRect.GetRect` | [unregistered] | Function |  |
| `CPdxFloatRect.Self` | [unregistered] | Function |  |
| `CPdxIntRect` |  | Type | Rectangle within an 2D integer space. |
| `CPdxIntRect.AccessSelf` | [unregistered] | Function |  |
| `CPdxIntRect.GetRect` | [unregistered] | Function |  |
| `CPdxIntRect.Self` | [unregistered] | Function |  |
| `CString` |  | Type |  |
| `CString.AccessSelf` | [unregistered] | Function |  |
| `CString.GetString` | [unregistered] | Function |  |
| `CString.Self` | [unregistered] | Function |  |
| `CurrentAndMaxToProgressbarValueInt32( Arg0, Arg1 )` | float | Global function | Get the progress (0.0-100.0) of CurrentArg to MaxArg. Value is clamped between 0 and 100. |
| `CUTF8String` |  | Type |  |
| `CUTF8String.AccessSelf` | [unregistered] | Function |  |
| `CUTF8String.GetString` | [unregistered] | Function |  |
| `CUTF8String.Self` | [unregistered] | Function |  |
| `CVector2f` |  | Type | Type containing two float numbers. |
| `CVector2f.AccessSelf` | [unregistered] | Function |  |
| `CVector2f.GetVector` | [unregistered] | Function |  |
| `CVector2f.Self` | [unregistered] | Function |  |
| `CVector2i` |  | Type | Type containing two integer numbers. |
| `CVector2i.AccessSelf` | [unregistered] | Function |  |
| `CVector2i.GetVector` | [unregistered] | Function |  |
| `CVector2i.Self` | [unregistered] | Function |  |
| `CVector3f` |  | Type | Type containing three float numbers. |
| `CVector3f.AccessSelf` | [unregistered] | Function |  |
| `CVector3f.GetVector` | [unregistered] | Function |  |
| `CVector3f.Self` | [unregistered] | Function |  |
| `CVector3i` |  | Type | Type containing three integer numbers. |
| `CVector3i.AccessSelf` | [unregistered] | Function |  |
| `CVector3i.GetVector` | [unregistered] | Function |  |
| `CVector3i.Self` | [unregistered] | Function |  |
| `CVector4f` |  | Type | Type containing four float numbers. |
| `CVector4f.AccessSelf` | [unregistered] | Function |  |
| `CVector4f.GetVector` | [unregistered] | Function |  |
| `CVector4f.Self` | [unregistered] | Function |  |
| `CVector4i` |  | Type | Type containing four integer numbers. |
| `CVector4i.AccessSelf` | [unregistered] | Function |  |
| `CVector4i.GetVector` | [unregistered] | Function |  |
| `CVector4i.Self` | [unregistered] | Function |  |
| `DataModelFirst( Arg0, Arg1 )` | [unregistered] | Global function |  |
| `DataModelHasItems( Arg0 )` | bool | Global function |  |
| `DataModelLast( Arg0, Arg1 )` | [unregistered] | Global function |  |
| `DataModelRepeatedItem( Arg0 )` | [unregistered] | Global function | The data function used for mocking empty list items, requires int32 (number of items) as a parameter. Example:… |
| `DataModelSkipFirst( Arg0, Arg1 )` | [unregistered] | Global function |  |
| `DataModelSkipLast( Arg0, Arg1 )` | [unregistered] | Global function |  |
| `DataModelSubSpan( Arg0, Arg1, Arg2 )` | [unregistered] | Global function |  |
| `Date` |  | Type | Jomini Historical Date |
| `Date.AccessSelf` | [unregistered] | Function |  |
| `Date.Self` | [unregistered] | Function |  |
| `Divide_CFixedPoint( Arg0, Arg1 )` | CFixedPoint | Global function |  |
| `Divide_CVector2f( Arg0, Arg1 )` | CVector2f | Global function |  |
| `Divide_float( Arg0, Arg1 )` | float | Global function |  |
| `Divide_int32( Arg0, Arg1 )` | int32 | Global function |  |
| `Divide_int64( Arg0, Arg1 )` | int64 | Global function |  |
| `Divide_uint32( Arg0, Arg1 )` | uint32 | Global function |  |
| `Divide_uint64( Arg0, Arg1 )` | uint64 | Global function |  |
| `double` |  | Type | Number stored with `double float` precision. |
| `double.AccessSelf` | [unregistered] | Function |  |
| `double.Self` | [unregistered] | Function |  |
| `DoubleToFloat( Arg0 )` | float | Global function |  |
| `EqualTo_CFixedPoint( Arg0, Arg1 )` | bool | Global function |  |
| `EqualTo_CVector2f( Arg0, Arg1 )` | bool | Global function |  |
| `EqualTo_float( Arg0, Arg1 )` | bool | Global function |  |
| `EqualTo_int32( Arg0, Arg1 )` | bool | Global function |  |
| `EqualTo_int64( Arg0, Arg1 )` | bool | Global function |  |
| `EqualTo_string( Arg0, Arg1 )` | bool | Global function |  |
| `EqualTo_uint32( Arg0, Arg1 )` | bool | Global function |  |
| `EqualTo_uint64( Arg0, Arg1 )` | bool | Global function |  |
| `FixedPointToFloat( Arg0 )` | float | Global function |  |
| `FixedPointToInt( Arg0 )` | int32 | Global function |  |
| `FixedPointToProgressbarValue( Arg0 )` | float | Global function | Convert to a progress percentage value (by multiplying with 100, unclamped). |
| `float` |  | Type | Number stored with `float` precision. |
| `float.AccessSelf` | [unregistered] | Function |  |
| `float.Self` | [unregistered] | Function |  |
| `GetDataModelSize( Arg0 )` | int32 | Global function |  |
| `GetDefine( Arg0, Arg1 )` | [unregistered] | Global function |  |
| `GetDefineAtIndex( Arg0, Arg1, Arg2 )` | [unregistered] | Global function |  |
| `GetGameVersionDisplay` | CString | Global function |  |
| `GetLocalizedDefine( Arg0, Arg1 )` | CString | Global function |  |
| `GetLocalizedDefineAtIndex( Arg0, Arg1, Arg2 )` | CString | Global function |  |
| `GetNumberAbove_int32( Arg0, Arg1 )` | int32 | Global function |  |
| `GetString_CPdxFloatRect( Arg0 )` | CString | Global function |  |
| `GetString_CPdxIntRect( Arg0 )` | CString | Global function |  |
| `GetString_CVector2f( Arg0 )` | CString | Global function |  |
| `GetString_CVector2i( Arg0 )` | CString | Global function |  |
| `GetString_CVector3f( Arg0 )` | CString | Global function |  |
| `GetString_CVector3i( Arg0 )` | CString | Global function |  |
| `GetString_CVector4f( Arg0 )` | CString | Global function |  |
| `GetString_CVector4i( Arg0 )` | CString | Global function |  |
| `GetVariableSystem` | VariableSystem | Global promote | Access the global variable system |
| `GreaterThan_CFixedPoint( Arg0, Arg1 )` | bool | Global function |  |
| `GreaterThan_float( Arg0, Arg1 )` | bool | Global function |  |
| `GreaterThan_int32( Arg0, Arg1 )` | bool | Global function |  |
| `GreaterThan_int64( Arg0, Arg1 )` | bool | Global function |  |
| `GreaterThan_uint32( Arg0, Arg1 )` | bool | Global function |  |
| `GreaterThan_uint64( Arg0, Arg1 )` | bool | Global function |  |
| `GreaterThanOrEqualTo_CFixedPoint( Arg0, Arg1 )` | bool | Global function |  |
| `GreaterThanOrEqualTo_float( Arg0, Arg1 )` | bool | Global function |  |
| `GreaterThanOrEqualTo_int32( Arg0, Arg1 )` | bool | Global function |  |
| `GreaterThanOrEqualTo_int64( Arg0, Arg1 )` | bool | Global function |  |
| `GreaterThanOrEqualTo_uint32( Arg0, Arg1 )` | bool | Global function |  |
| `GreaterThanOrEqualTo_uint64( Arg0, Arg1 )` | bool | Global function |  |
| `InDebugMode` | bool | Global function | Is the game in Debug mode? |
| `InReleaseMode` | bool | Global function | Is the game in Release mode? |
| `int16` |  | Type |  |
| `int16.AccessSelf` | [unregistered] | Function |  |
| `int16.GetInt` | [unregistered] | Function |  |
| `int16.Self` | [unregistered] | Function |  |
| `int32` |  | Type |  |
| `int32.AccessSelf` | [unregistered] | Function |  |
| `int32.GetInt` | [unregistered] | Function |  |
| `int32.Self` | [unregistered] | Function |  |
| `int64` |  | Type |  |
| `int64.AccessSelf` | [unregistered] | Function |  |
| `int64.GetInt` | [unregistered] | Function |  |
| `int64.Self` | [unregistered] | Function |  |
| `int8` |  | Type |  |
| `int8.AccessSelf` | [unregistered] | Function |  |
| `int8.GetInt` | [unregistered] | Function |  |
| `int8.Self` | [unregistered] | Function |  |
| `IntToFixedPoint( Arg0 )` | float | Global function |  |
| `IntToFloat( Arg0 )` | float | Global function |  |
| `IntToFrameIndex( Arg0 )` | int32 | Global function | Adds 1 to Arg1. |
| `IntToUnsigned( Arg0 )` | float | Global function |  |
| `IsDataModelEmpty( Arg0 )` | bool | Global function |  |
| `IsEven_int32( Arg0 )` | bool | Global function |  |
| `IsEven_int64( Arg0 )` | bool | Global function |  |
| `IsEven_uint32( Arg0 )` | bool | Global function |  |
| `IsEven_uint64( Arg0 )` | bool | Global function |  |
| `IsOdd_int32( Arg0 )` | bool | Global function |  |
| `IsOdd_int64( Arg0 )` | bool | Global function |  |
| `IsOdd_uint32( Arg0 )` | bool | Global function |  |
| `IsOdd_uint64( Arg0 )` | bool | Global function |  |
| `JoinText( Arg0, Arg1, Arg2 )` | CString | Global function |  |
| `LessThan_CFixedPoint( Arg0, Arg1 )` | bool | Global function |  |
| `LessThan_float( Arg0, Arg1 )` | bool | Global function |  |
| `LessThan_int32( Arg0, Arg1 )` | bool | Global function |  |
| `LessThan_int64( Arg0, Arg1 )` | bool | Global function |  |
| `LessThan_uint32( Arg0, Arg1 )` | bool | Global function |  |
| `LessThan_uint64( Arg0, Arg1 )` | bool | Global function |  |
| `LessThanOrEqualTo_CFixedPoint( Arg0, Arg1 )` | bool | Global function |  |
| `LessThanOrEqualTo_float( Arg0, Arg1 )` | bool | Global function |  |
| `LessThanOrEqualTo_int32( Arg0, Arg1 )` | bool | Global function |  |
| `LessThanOrEqualTo_int64( Arg0, Arg1 )` | bool | Global function |  |
| `LessThanOrEqualTo_uint32( Arg0, Arg1 )` | bool | Global function |  |
| `LessThanOrEqualTo_uint64( Arg0, Arg1 )` | bool | Global function |  |
| `Localize( Arg0 )` | CString | Global function |  |
| `Max_CFixedPoint( Arg0, Arg1 )` | CFixedPoint | Global function |  |
| `Max_CVector2f( Arg0, Arg1 )` | CVector2f | Global function |  |
| `Max_float( Arg0, Arg1 )` | float | Global function |  |
| `Max_int32( Arg0, Arg1 )` | int32 | Global function |  |
| `Max_int64( Arg0, Arg1 )` | int64 | Global function |  |
| `Max_uint32( Arg0, Arg1 )` | uint32 | Global function |  |
| `Max_uint64( Arg0, Arg1 )` | uint64 | Global function |  |
| `Min_CFixedPoint( Arg0, Arg1 )` | CFixedPoint | Global function |  |
| `Min_CVector2f( Arg0, Arg1 )` | CVector2f | Global function |  |
| `Min_float( Arg0, Arg1 )` | float | Global function |  |
| `Min_int32( Arg0, Arg1 )` | int32 | Global function |  |
| `Min_int64( Arg0, Arg1 )` | int64 | Global function |  |
| `Min_uint32( Arg0, Arg1 )` | uint32 | Global function |  |
| `Min_uint64( Arg0, Arg1 )` | uint64 | Global function |  |
| `Modulo_int32( Arg0, Arg1 )` | int32 | Global function |  |
| `Modulo_int64( Arg0, Arg1 )` | int64 | Global function |  |
| `Modulo_uint32( Arg0, Arg1 )` | uint32 | Global function |  |
| `Modulo_uint64( Arg0, Arg1 )` | uint64 | Global function |  |
| `Multiply_CFixedPoint( Arg0, Arg1 )` | CFixedPoint | Global function |  |
| `Multiply_CVector2f( Arg0, Arg1 )` | CVector2f | Global function |  |
| `Multiply_float( Arg0, Arg1 )` | float | Global function |  |
| `Multiply_int32( Arg0, Arg1 )` | int32 | Global function |  |
| `Multiply_int64( Arg0, Arg1 )` | int64 | Global function |  |
| `Multiply_uint32( Arg0, Arg1 )` | uint32 | Global function |  |
| `Multiply_uint64( Arg0, Arg1 )` | uint64 | Global function |  |
| `Nbsp` | CString | Global function |  |
| `Negate_CFixedPoint( Arg0 )` | CFixedPoint | Global function |  |
| `Negate_float( Arg0 )` | float | Global function |  |
| `Negate_int32( Arg0 )` | int32 | Global function |  |
| `Negate_int64( Arg0 )` | int64 | Global function |  |
| `Not( Arg0 )` | bool | Global function | If the argument is not true |
| `NotEqualTo_CFixedPoint( Arg0, Arg1 )` | bool | Global function |  |
| `NotEqualTo_CVector2f( Arg0, Arg1 )` | bool | Global function |  |
| `NotEqualTo_float( Arg0, Arg1 )` | bool | Global function |  |
| `NotEqualTo_int32( Arg0, Arg1 )` | bool | Global function |  |
| `NotEqualTo_int64( Arg0, Arg1 )` | bool | Global function |  |
| `NotEqualTo_uint32( Arg0, Arg1 )` | bool | Global function |  |
| `NotEqualTo_uint64( Arg0, Arg1 )` | bool | Global function |  |
| `ObjectsEqual( Arg0, Arg1 )` | bool | Global function |  |
| `Or( Arg0, Arg1 )` | bool | Global function | If either argument is true |
| `Or3( Arg0, Arg1, Arg2 )` | bool | Global function | If any of 3 arguments are true |
| `Or4( Arg0, Arg1, Arg2, Arg3 )` | bool | Global function | If any of 4 arguments are true |
| `Or5( Arg0, Arg1, Arg2, Arg3, Arg4 )` | bool | Global function | If any of 5 arguments are true |
| `Or6( Arg0, Arg1, Arg2, Arg3, Arg4, Arg5 )` | bool | Global function | If any of 6 arguments are true |
| `Or7( Arg0, Arg1, Arg2, Arg3, Arg4, Arg5, Arg6 )` | bool | Global function | If any of 7 arguments are true |
| `Or8( Arg0, Arg1, Arg2, Arg3, Arg4, Arg5, Arg6, Arg7 )` | bool | Global function | If any of 8 arguments are true |
| `OrEvalAll( Arg0, Arg1 )` | bool | Global function | Evaluates all arguments and then checks if either argument is true. Using 'Or' is preferred. |
| `Playable` |  | Type | Jomini Playable |
| `Playable.AccessSelf` | [unregistered] | Function |  |
| `Playable.GetCountry` | Country | Promote |  |
| `Playable.GetJominiPlayableCountryRef` | [unregistered] | Function |  |
| `Playable.GetJominiPlayableCountryRef` | Country | Promote |  |
| `Playable.Self` | [unregistered] | Function |  |
| `Select_CFixedPoint( Arg0, Arg1, Arg2 )` | CFixedPoint | Global function |  |
| `Select_CString( Arg0, Arg1, Arg2 )` | CString | Global function |  |
| `Select_CVector2f( Arg0, Arg1, Arg2 )` | CVector2f | Global function |  |
| `Select_CVector2i( Arg0, Arg1, Arg2 )` | CVector2i | Global function |  |
| `Select_CVector3f( Arg0, Arg1, Arg2 )` | CVector3f | Global function |  |
| `Select_CVector3i( Arg0, Arg1, Arg2 )` | CVector3i | Global function |  |
| `Select_CVector4f( Arg0, Arg1, Arg2 )` | CVector4f | Global function |  |
| `Select_CVector4i( Arg0, Arg1, Arg2 )` | CVector4i | Global function |  |
| `Select_float( Arg0, Arg1, Arg2 )` | float | Global function |  |
| `Select_int16( Arg0, Arg1, Arg2 )` | int16 | Global function |  |
| `Select_int32( Arg0, Arg1, Arg2 )` | int32 | Global function |  |
| `Select_int64( Arg0, Arg1, Arg2 )` | int64 | Global function |  |
| `Select_int8( Arg0, Arg1, Arg2 )` | int8 | Global function |  |
| `Select_uint16( Arg0, Arg1, Arg2 )` | uint16 | Global function |  |
| `Select_uint32( Arg0, Arg1, Arg2 )` | uint32 | Global function |  |
| `Select_uint64( Arg0, Arg1, Arg2 )` | uint64 | Global function |  |
| `Select_uint8( Arg0, Arg1, Arg2 )` | uint8 | Global function |  |
| `SelectLocalization( Arg0, Arg1, Arg2 )` | CString | Global function |  |
| `StringIsEmpty( Arg0 )` | bool | Global function |  |
| `Subtract_CFixedPoint( Arg0, Arg1 )` | CFixedPoint | Global function |  |
| `Subtract_CVector2f( Arg0, Arg1 )` | CVector2f | Global function |  |
| `Subtract_float( Arg0, Arg1 )` | float | Global function |  |
| `Subtract_int32( Arg0, Arg1 )` | int32 | Global function |  |
| `Subtract_int64( Arg0, Arg1 )` | int64 | Global function |  |
| `Subtract_uint32( Arg0, Arg1 )` | uint32 | Global function |  |
| `Subtract_uint64( Arg0, Arg1 )` | uint64 | Global function |  |
| `TagObject` |  | Type | Base class for objects featuring custom tags |
| `TagObject.AccessSelf` | [unregistered] | Function |  |
| `TagObject.CanShowTags` | bool | Function |  |
| `TagObject.Self` | [unregistered] | Function |  |
| `TagObject.ShowTags` | CString | Function |  |
| `ToString_FixedPoint( Arg0, Arg1 )` | CString | Global function |  |
| `ToString_int16( Arg0 )` | CString | Global function |  |
| `ToString_int32( Arg0 )` | CString | Global function |  |
| `ToString_int64( Arg0 )` | CString | Global function |  |
| `ToString_int8( Arg0 )` | CString | Global function |  |
| `ToString_uint16( Arg0 )` | CString | Global function |  |
| `ToString_uint32( Arg0 )` | CString | Global function |  |
| `ToString_uint64( Arg0 )` | CString | Global function |  |
| `ToString_uint8( Arg0 )` | CString | Global function |  |
| `uint16` |  | Type |  |
| `uint16.AccessSelf` | [unregistered] | Function |  |
| `uint16.GetUint` | [unregistered] | Function |  |
| `uint16.Self` | [unregistered] | Function |  |
| `uint32` |  | Type |  |
| `uint32.AccessSelf` | [unregistered] | Function |  |
| `uint32.GetUint` | [unregistered] | Function |  |
| `uint32.Self` | [unregistered] | Function |  |
| `uint64` |  | Type |  |
| `uint64.AccessSelf` | [unregistered] | Function |  |
| `uint64.GetUint` | [unregistered] | Function |  |
| `uint64.Self` | [unregistered] | Function |  |
| `uint8` |  | Type |  |
| `uint8.AccessSelf` | [unregistered] | Function |  |
| `uint8.GetUint` | [unregistered] | Function |  |
| `uint8.Self` | [unregistered] | Function |  |
| `VariableSystem` |  | Type |  |
| `VariableSystem.AccessSelf` | [unregistered] | Function |  |
| `VariableSystem.Clear( Arg0 )` | void | Function |  |
| `VariableSystem.ClearIf( Arg0, Arg1 )` | void | Function |  |
| `VariableSystem.ClearMultiple( Arg0, Arg1 )` | void | Function |  |
| `VariableSystem.Exists( Arg0 )` | bool | Function |  |
| `VariableSystem.Get( Arg0 )` | CString | Function |  |
| `VariableSystem.HasValue( Arg0, Arg1 )` | bool | Function |  |
| `VariableSystem.Self` | [unregistered] | Function |  |
| `VariableSystem.Set( Arg0, Arg1 )` | void | Function |  |
| `VariableSystem.SetIf( Arg0, Arg1, Arg2 )` | void | Function |  |
| `VariableSystem.SetOrToggle( Arg0, Arg1 )` | void | Function |  |
| `VariableSystem.Toggle( Arg0 )` | bool | Function |  |
| `void` |  | Type | Unspecified type / empty type. |
| `void.AccessSelf` | [unregistered] | Function |  |
| `void.Self` | [unregistered] | Function |  |
