`bool GetControllerState( vr::TrackedDeviceIndex_t unControllerDeviceIndex, vr::VRControllerState_t *pControllerState )`<br>
`bool GetControllerStateWithPose( TrackingUniverseOrigin eOrigin, vr::TrackedDeviceIndex_t unControllerDeviceIndex, vr::VRControllerState_t *pControllerState, TrackedDevicePose_t *pTrackedDevicePose )`

Fills the supplied struct with the current state of the controller.

* `vr::TrackedDeviceIndex_t unControllerDeviceIndex` - The tracked device index of the controller to get the state of.
* `vr::VRControllerState_t *pControllerState` - A struct to fill with the controller state.
* `TrackingUniverseOrigin eOrigin` - The tracking coordinate system to return the pose in.
* `TrackedDevicePose_t *pTrackedDevicePose` - A pose struct to fill with the pose of the controller when the last button event occurred.

Fills the supplied struct with the current state of the controller. Returns false if the controller index is invalid or if state is not available from the controller for some reason.

GetControllerStateWithPose will also fill the provided pose struct with the pose of 
the controller when the controller button state was updated most recently. Use this form if you need a precise controller pose as input to your application when the user presses or releases a button. This pose is always older than the current time, so it should not be used for rendering.

**Controller state structs**

	/** Identifies what kind of axis is on the controller at index n. Read this type 
	* with pVRSystem->Get( nControllerDeviceIndex, Prop_Axis0Type_Int32 + n );
	*/
	enum EVRControllerAxisType
	{
		k_eControllerAxis_None = 0,
		k_eControllerAxis_TrackPad = 1,
		k_eControllerAxis_Joystick = 2,
		k_eControllerAxis_Trigger = 3, // Analog trigger data is in the X axis
	};


	/** contains information about one axis on the controller */
	struct VRControllerAxis_t
	{
		float x; // Ranges from -1.0 to 1.0 for joysticks and track pads. Ranges from 0.0 to 1.0 for triggers were 0 is fully released.
		float y; // Ranges from -1.0 to 1.0 for joysticks and track pads. Is always 0.0 for triggers.
	};


	/** the number of axes in the controller state */
	static const uint32_t k_unControllerStateAxisCount = 5;


	/** Holds all the state of a controller at one moment in time. */
	struct VRControllerState001_t
	{
		// If packet num matches that on your prior call, then the controller state hasn't been changed since 
		// your last call and there is no need to process it
		uint32_t unPacketNum;

		// bit flags for each of the buttons. Use ButtonMaskFromId to turn an ID into a mask
		uint64_t ulButtonPressed;
		uint64_t ulButtonTouched;

		// Axis data for the controller's analog inputs
		VRControllerAxis_t rAxis[ k_unControllerStateAxisCount ];
	};


	typedef VRControllerState001_t VRControllerState_t;
