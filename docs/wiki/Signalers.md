# Sound Signaler
A sound signaler will signal certain states of the Machine or a JobProcessor. If e.g. a placement or pickup fails it will play an error sound. It can also play a sound when the Job is complete.

## Configuration
The SoundSignaler can be configured in the machine.xml:

```
<signalers>
    <signaler class="org.openpnp.machine.reference.signaler.SoundSignaler" id="b7980099-1992-42b4-b38a-2f8ee6b52eb8" name="Sound Signaler" enable-error-sound="true" enable-finished-sound="true"/>
</signalers>
```

### Disabling Sounds
If you want to disable the sounds you can set the attributes on the the SoundSignaler to false:
`enable-error-sound="false"`
`enable-success-sound="false"`

### Custom sound files
It is possible to use custom sound files by placing an `error.wav` or `success.wav` file in a `sounds` subdirectory in the [configuration directory](https://github.com/openpnp/openpnp/wiki/FAQ#how-do-i-use-a-different-config-directory).

# Actuator Signaler
An actuator signaler will cause an actuator to be turned on when a configured job or machine state is reached. They will stay on as long as the state is active.

## Configuration
The actuator signaler can be configured in the machine.xml:

```
<signalers>
    <signaler class="org.openpnp.machine.reference.signaler.ActuatorSignaler" id="446118b2-991d-11e6-9f33-a24fc0d9649c" name="Actuator Signaler" actuator-id="5b8a8cbd-d6c1-4324-af8b-eba5f0444622" job-state="ERROR" machine-state="ERROR"/>
</signalers>
```
Note that you if you create the signaller in the UI, you will need to edit the machine.xml to add the appropriate actuator-id, job-state and machine-state, as the signaler and it's actuator are not automatically connected via the UI.

The valid options for job-state are:
* STOPPED
* RUNNING
* ERROR
* FINISHED

The valid options for machine-state are:
* ERROR


