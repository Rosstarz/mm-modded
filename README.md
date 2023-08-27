## Assembly-CSharp.dll Changes:

### TODO
- grid showcase scrollspeed increase
- 'Auto On' option always automatically on
- rotate camera option
- add inability to click on menu

#### Use AI for drivers
ConsoleCommands -> useAIForDrivers
```
public static bool useAIForDrivers = true;
public static bool useAIForPitting = true;
```

#### Auto-setup
SessionSetup -> GetSetupOutput
```
if (this.mVehicle.isPlayerDriver)
{
	this.mAISetupOutput = new SessionSetup.SetupOutput();
	this.SetAISetupWithinRange(0.69f, 1f);
	outSetupOutput.aerodynamics = this.mAISetupOutput.aerodynamics;
	outSetupOutput.speedBalance = this.mAISetupOutput.speedBalance;
	outSetupOutput.handling = this.mAISetupOutput.handling;
	return;
}
if (this.mAISetupOutput == null)
{
	this.mAISetupOutput = new SessionSetup.SetupOutput();
	this.CreateAISetupForQualifyingAndRace();
}
outSetupOutput.aerodynamics = this.mAISetupOutput.aerodynamics;
outSetupOutput.speedBalance = this.mAISetupOutput.speedBalance;
outSetupOutput.handling = this.mAISetupOutput.handling;
```

#### Disable Dilemmas
- ChallengeManager -> AreDilemmasBlocked()
```
public bool AreDilemmasBlocked()
{
	return true;
	// return this.IsAttemptingChallenge() && this.currentChallenge.blockDilemmas;
}
```

#### More Crashes
CrashDirector -> OnSessionStarting()
```
switch (DesignDataManager.GetGameLength(false))
{
	case PrefGameRaceLength.Type.Short:
		this.mRealSafetyCarCount = 2;
		break;
	case PrefGameRaceLength.Type.Medium:
		this.mRealSafetyCarCount = 4;
		break;
	case PrefGameRaceLength.Type.Long:
		this.mRealSafetyCarCount = int.MaxValue;
		break;
}
```
```
if (random > 0.95f)
{
	i = 5;
}
else if (random > 0.85f)
{
	i = 4;
}
else if (random > 0.7f)
{
	i = 3;
}
else if (random > 0.45f)
{
	i = 2;
}
else if (random > 0.2f)
{
	i = 1;
}
else
{
	i = 0;
}
if (this.mSessionManager.currentSessionWeather.GetAverageWeather().rainType >= Weather.RainType.Heavy && i < 4)
{
	i += RandomUtility.GetRandom(1, 3);
}
```

#### More Lockup
- PathData -> CalculateLockUpZones()
```
float num = 150
```
- AILockUpBehaviour ->OnEnter
```
this.mLockUpDuration = (float)RandomUtility.GetRandom(1, 3)
```
- AILockUpBehaviour ->OnExit
```
float random = RandomUtility.GetRandom(0.03f, 0.09f)
```
- TyreLockUpDirector IsTyreLockUpViable
```
bool flag = Game.instance.sessionManager.flag == SessionManager.Flag.Chequered;
bool flag2 = inVehicle.timer.gapToAhead < 3.6f - inVehicle.driver.GetDriverStats().braking / 6f;
bool flag3 = Game.instance.sessionManager.currentSessionWeather.GetNormalizedTrackWater() > inVehicle.driver.GetDriverStats().adaptability / 21f && RandomUtility.GetRandom01() < 0.5f;
bool flag4 = Game.instance.sessionManager.currentSessionWeather.GetNormalizedTrackRubber() < (20.4f - inVehicle.driver.GetDriverStats().cornering) / 21f && RandomUtility.GetRandom01() < 0.5f;
bool flag5 = Game.instance.sessionManager.GetNormalizedSessionTime() > inVehicle.driver.GetDriverStats().fitness / 22f;
return inVehicle.speed <= GameUtility.MilesPerHourToMetersPerSecond(55f) && !isTutorialActiveInCurrentGameState && !inVehicle.behaviourManager.isOutOfRace && !flag && (flag2 & (flag3 || flag4 || flag5)) && inVehicle.sessionEvents.IsReadyTo(SessionEvents.EventType.LockUp);
float mFrequency = 45f
```

#### Fixing Flags
- Removed Sochi from OpenWheel
```
Locations.txt - get track numbers
\r\nBlack Sea,34,35,36,...
Championships.txt - replace track numbers
```
- Nationalities
```
Drivers.txt,Teams.txt
replace country name
```

### No Intro Skip
```
Replaced SplashSequence.bk2 and AttractIntro.bk2 with a zero length file from here :
https://mega.nz/#!fdhi3TRZ!1P_2GhzbqmjyRyhyXGK2qCIOv2AVtUhMs4rGY42kTX4
```
