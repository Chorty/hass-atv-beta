# Apple TV Beta Component

**TL;DR** If you try this component, read the changelog below to know what has
changed and what to test!

In tvOS 13, Apple dropped their old legacy protocols inherited from iTunes
(DAAP/DACP/DMAP) and now rely fully on their new protocol, Media Remote Protocol.
Or MRP for short. This broke the Apple TV component in Home Assistant as
`pyatv`, the underlying protocol, only supports the old legacy protocol stack.

I have put a lot of work into completing suport for MRP in `pyatv` and it is
starting to reach a point where it is usable. But there is still some work
left until it can be released and all changes integrated back into Home Assistant.
However, I want to give everyone a chance to try out the new component and come
with feedback, so I publish what will become the official component (that is
bundled with Home Assistant by default) here as a custom component so you can
try it out.

Issues and trouble reports should be reported in the `pyatv` repository:

[**>> Report issues here <<**](https://github.com/postlund/pyatv/issues)

## Features and limitations

Currently Home Assistant implements all features that `pyatv` suports for DMAP.
The intention is to implement the same features for MRP first and add additional
features after that. The feature list is available
[here](https://postlund.github.io/pyatv/).

Other limitations as follows:

* As Home Assistant does not support zeroconf for custom components, auto-discovery
  will not work with this component. You will have to manually add your device
  via the Integrations page.

* This component will completely override the builtin component!

* Using YAML is not supported now, but will be added in the future.

### Changes

#### pyatv 0.4.0a10

Finally support for artwork!

Improvements to AirPlay to make it more reliable. Should hopefully fix #313.

#### pyatv 0.4.0a9

This is a small fix that makes idle state work for MRP (a device was never
shown as idle before).

#### pyatv 0.4.0a8

Fixes this issue:

    TypeError: Object of type MrpService is not JSON serializable

Remove your integration and add it again and it should work.

Leading zeros in PIN when pairing with AirPlay should work now. (#307)

#### pystv 0.4.0a7

_Beware: this release might be a bit buggy, please help me hunt the bugs down._

Fixes problems with leading zeros in MRP pairing:

https://github.com/postlund/pyatv/issues/291

Hopefully AirPlay streaming works as expected now (please try):

https://github.com/postlund/pyatv/issues/266

Re-connect logic is in place, so re-connections are made automatically.
Might be a bit spammy, will change that later.

Turn on and off should work now. Please note that this *only* means that
"turn off" disconnects from the device so it appears as off in Home Assistant.
It does **not** turn off your Apple TV.

I added the "start off" option as well. You can configure it via Options
by selecting your Apple TV from the Integrations page.

#### pyatv 0.4.0a6

Fixes this problem:

```python
packages/pyatv/mrp/pairing.py", line 32, in close await self.connection.close() TypeError: object NoneType can't be used in 'await' expression
```

#### pyatv 0.4.0a5

Fixes this problem:

```python
AttributeError: 'Credentials' object has no attribute 'split'
```

In case of problems, remove the Apple TV entity and re-pair.

## Installing

I recommend that you install [HACS](https://hacs.xyz/) and add this repository
to it. That way you get updates automatically. But you can just copy and add
files the old fashined way as well.

## Setting up

Head over to that Integrations page and add an Apple TV from there. You have to
provide either the name of a device, its IP-address or a unique identifier
(that you got via `atvremote scan`). If you are unsure about what to enter, just
type something, press submit and suggestions will be presented for you.

## Debug logs

If you run into problems, please (please, **please**) make sure you include debug
logs. It is really hard to debug without them. You enable them like this:

```yaml
logger:
  logs:
    pyatv: debug
```

## Finally...

Remember, this is beta software. Features are not fully developed yet, things
will not work, etc. If you try it out, I would be *very* grateful if you reported
any issues you encounter. It helps me iron out bugs and making the integration
stable before submitting it to Home Assistant. It's a win-win in the end, really.