Enable Editor Preferences > General > Miscellaneous > Developer Tools > Display UE Extension Points to see the names of various elements in the Unreal Editor UI that can be extended.
These names are passed to `FExtender::AddToolBarExtension`.
The editor must be restarted for this change to take effect.


```c++
/*  */YourEditorCommands = MakeShareable(new FUICommandList);
// MAP ACTIONS TO YOUR COMMANDS
YourEditorCommands->MapAction(
   FYourEditorCommands::Get().YourCommand,
   FExecuteAction::CreateStatic(&FYourEditorCommands::OnClickedYourCommand),
   FCanExecuteAction());

FLevelEditorModule& levelEditorModule = FModuleManager::LoadModuleChecked<FLevelEditorModule>("LevelEditor");

TSharedPtr<FExtender> toolbarExtender = MakeShareable(new FExtender);
toolbarExtender->AddToolBarExtension("Game", EExtensionHook::After, YourEditorCommands,
   FToolBarExtensionDelegate::CreateRaw(this, &FYourEditorModule::CreateYourEditorCommmandSettings));

levelEditorModule.GetToolBarExtensibilityManager()->AddExtender(toolbarExtender);

levelEditorModule.GetGlobalLevelEditorActions()->Append(YourEditorCommands.ToSharedRef());

void FYourEditorModule::CreateYourEditorCommmandSettings(FToolBarBuilder& Builder)
{
   Builder.BeginSection(FName("YourSection"));
   Builder.AddComboButton(
       FUIAction(),
       FOnGetContent::CreateRaw(this, &FYourEditorModule::CreateYourEditorCommmandWidget),
       LOCTEXT("YourEditor_Title", "Some Title"),
       LOCTEXT("YourEditor_Tooltip", "Some tooltip."),
       FSlateIcon(FYourEditorStyle::GetStyleSetName(), "Your.Icon.Name"));
   Builder.EndSection();
}
```
