Added to PlayerController:
```C#
	using System.IO;
```



```C#
public bool getButton(InputGamepadButton button)
{
	if (InputGamePadMgr.rewired == null)
	{
		InputGamePadMgr.SetupRewired();
	}
	if (InputGamePadMgr.rewired == null)
	{
		return false;
	}
	if (InputGamePadMgr.rewired.GetButton(InputGamePadMgr.GamePadToRewired(button)))
	{
		return true;
	}
	if (button == InputGamepadButton.Action)
	{
		if (Input.GetKey(PlayerController.Jump))
		{
			return true;
		}
	}
	else if (button == InputGamepadButton.Back)
	{
		if (Input.GetKey(PlayerController.Red))
		{
			return true;
		}
	}
	else if (button == InputGamepadButton.AltAction)
	{
		if (Input.GetKey(PlayerController.Blue))
		{
			return true;
		}
	}
	else if (button == InputGamepadButton.Menu)
	{
		if (Input.GetKey(PlayerController.Yellow))
		{
			return true;
		}
	}
	else if (button == InputGamepadButton.Start)
	{
		if (Input.GetKey(PlayerController.Pause))
		{
			return true;
		}
	}
	else if (button == InputGamepadButton.Select)
	{
		if (Input.GetKey(PlayerController.Pause))
		{
			return true;
		}
	}
	else if (button == InputGamepadButton.RB)
	{
		if (Input.GetKey(KeyCode.AltGr) || Input.GetKey(KeyCode.RightAlt))
		{
			return true;
		}
	}
	else if (button == InputGamepadButton.LB && Input.GetKey(KeyCode.LeftAlt))
	{
		return true;
	}
	return false;
}
 


	public static float GetAxisRaw(string AxisName)
{
	if (InputGamePadMgr.rewired == null)
	{
		InputGamePadMgr.SetupRewired();
	}
	if (InputGamePadMgr.rewired == null)
	{
		return 0f;
	}
	if (AxisName != "Horizontal" && AxisName != "Vertical")
	{
		return InputGamePadMgr.rewired.GetAxisRaw(InputGamePadMgr.GamePadToRewired(AxisName));
	}
	if (Application.platform == RuntimePlatform.OSXEditor || Application.platform == RuntimePlatform.OSXPlayer || Application.platform == RuntimePlatform.OSXWebPlayer)
	{
		if (AxisName == "DPadY")
		{
			return (!Input.GetKey(KeyCode.JoystickButton5)) ? ((!Input.GetKeyDown(KeyCode.JoystickButton6)) ? 0f : -1f) : 1f;
		}
		if (AxisName == "DPadX")
		{
			return (!Input.GetKey(KeyCode.JoystickButton8)) ? ((!Input.GetKeyDown(KeyCode.JoystickButton7)) ? 0f : -1f) : 1f;
		}
		return Input.GetAxisRaw(AxisName + "_OSX" + (((LanguageMgr.GetLangEnum() == GameLanguages.French && !InputGamePadMgr.ForceWASD) || (!(AxisName == "Vertical") && !(AxisName == "Horizontal"))) ? string.Empty : "_QW"));
	}
	else
	{
		if (Application.platform != RuntimePlatform.WindowsPlayer && Application.platform != RuntimePlatform.WindowsEditor && Application.platform != RuntimePlatform.WindowsWebPlayer && Application.platform != RuntimePlatform.MetroPlayerARM && Application.platform != RuntimePlatform.MetroPlayerX64 && Application.platform != RuntimePlatform.MetroPlayerX86 && Application.platform != RuntimePlatform.LinuxPlayer)
		{
			return 0f;
		}
		if ((AxisName == "DPadX" || AxisName == "DPadY") && Input.GetJoystickNames().Length > 0 && Input.GetJoystickNames()[0].Contains("Xbox One For Windows") && SystemInfo.operatingSystem.Contains("Windows 10"))
		{
			return Input.GetAxisRaw(AxisName + "_Win10");
		}
		if (InputGamePadMgr.IsPS4Controller() && (AxisName.Contains("RightStick") || AxisName.Contains("DPad") || AxisName.Contains("Trigger")))
		{
			return Input.GetAxisRaw(AxisName + "_PS4");
		}
		if (AxisName == "Vertical")
		{
			if (Input.GetKey(KeyCode.DownArrow) || (Input.GetKey(KeyCode.S) && KeyCode.S != PlayerController.Blue && KeyCode.S != PlayerController.Yellow && KeyCode.S != PlayerController.Jump && KeyCode.S != PlayerController.Red))
			{
				return 1f;
			}
			if (Input.GetKey(KeyCode.UpArrow) || (Input.GetKey(KeyCode.W) && KeyCode.W != PlayerController.Blue && KeyCode.W != PlayerController.Yellow && KeyCode.W != PlayerController.Jump && KeyCode.W != PlayerController.Red))
			{
				return -1f;
			}
			return 0f;
		}
		else
		{
			if (Input.GetKey(KeyCode.LeftArrow) || (Input.GetKey(KeyCode.A) && KeyCode.A != PlayerController.Blue && KeyCode.A != PlayerController.Yellow && KeyCode.A != PlayerController.Jump && KeyCode.A != PlayerController.Red))
			{
				return 1f;
			}
			if (Input.GetKey(KeyCode.RightArrow) || (Input.GetKey(KeyCode.D) && KeyCode.D != PlayerController.Blue && KeyCode.D != PlayerController.Yellow && KeyCode.D != PlayerController.Jump && KeyCode.D != PlayerController.Red))
			{
				return -1f;
			}
			return 0f;
		}
		if (Input.GetJoystickNames().Length != 0)
		{
			return Input.GetAxisRaw(AxisName + (((LanguageMgr.GetLangEnum() == GameLanguages.French && !InputGamePadMgr.ForceWASD) || (!(AxisName == "Vertical") && !(AxisName == "Horizontal"))) ? string.Empty : "_QW"));
		}
	}
}
```
replaced all instances of InputGamePadMgr.getButton with getButton and InputGamePadMgr.GetAxisRaw with GetAxisRaw


Added in Awake() of PlayerController:
```C#
	StreamReader streamReader = new StreamReader(Application.dataPath + "/Input.txt");
	string fileContents = streamReader.ReadToEnd();
	streamReader.Close();
	string[] lines = fileContents.Split(new char[]
	{
		"\n"[0]
	});
	PlayerController.Jump = (KeyCode)Enum.Parse(typeof(KeyCode), lines[0]);
	PlayerController.Blue = (KeyCode)Enum.Parse(typeof(KeyCode), lines[1]);
	PlayerController.Red = (KeyCode)Enum.Parse(typeof(KeyCode), lines[2]);
	PlayerController.Yellow = (KeyCode)Enum.Parse(typeof(KeyCode), lines[3]);
	PlayerController.Pause = (KeyCode)Enum.Parse(typeof(KeyCode), lines[4]);
```



Added Variables in PlayerController:
```C#
	private static KeyCode Jump;

	
	private static KeyCode Blue;

	
	private static KeyCode Red;

	
	private static KeyCode Yellow;

	
	private static KeyCode Pause;
```