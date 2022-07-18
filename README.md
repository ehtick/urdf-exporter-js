# urdf-exporter-js

[![npm version](https://img.shields.io/npm/v/urdf-exporter.svg?style=flat-square)](https://www.npmjs.com/package/urdf-exporter)
[![lgtm code quality](https://img.shields.io/lgtm/grade/javascript/g/gkjohnson/urdf-exporter-js.svg?style=flat-square&label=code-quality)](https://lgtm.com/projects/g/gkjohnson/urdf-exporter-js/)
[![build](https://img.shields.io/github/workflow/status/gkjohnson/urdf-exporter-js/Node.js%20CI?style=flat-square&label=build)](https://github.com/gkjohnson/urdf-exporter-js/actions)
[![github](https://flat.badgen.net/badge/icon/github?icon=github&label)](https://github.com/gkjohnson/urdf-exporter-js/)
[![twitter](https://flat.badgen.net/twitter/follow/garrettkjohnson)](https://twitter.com/garrettkjohnson)
[![sponsors](https://img.shields.io/github/sponsors/gkjohnson?style=flat-square&color=1da1f2)](https://github.com/sponsors/gkjohnson/)

Utility for exporting a three.js object hierarchy as a URDF.

# Use

```js
import { URDFExporter } from 'urdf-exporter';
import { STLExporter } from 'three/examples/jsm/exporters/STLExporter.js';

let urdfModel;
// ... create a model using the URDF classes for export

const models = {};
const exporter = new URDFExporter();
exporter.processGeometryCallback = ( model, link ) => {

	const name = `${ link.name }_mesh.stl`;
	models[ name ] = new STLExporter().parse( model );
	return name;

};

urdfModel.updateMatrixWorld();
const urdf = exporter.parse( urdfModel );

// ... urdf content ready!

```


# API

## URDFExporter

### .indent

```js
indent = '\t' : string
```

The set of indentation characters to use.

### .processGeometryCallback

```js
processGeometryCallback : ( node : Object3D, link : URDFLink ) => string
```

The callback for to use when processing geometry. Geometry must be processed and cached in order to be exported. A file path is returned from the function.

### .parse

```js
parse( root : Object3D ) : string
```

Parses the object into a urdf file. Returns the URDF contents as a string. The hierarchy matrix world must be updated before calling this function.

## URDFLimit

### .upper

```js
upper = 0 : Number
```

### .lower

```js
lower = 0 : Number
```

### .velocity

```js
velocity = 0 : Number
```

### .effort

```js
effort = 0 : Number
```

## URDFInertialFrame

### .position

```js
position : Vector3
```

### .rotation

```js
rotation : Euler
```

### .mass

```js
mass = 0 : Number
```

### inertia

```js
inertial : Matrix3
```

## URDFLink

_extends THREE.Object3D_

### .inertial

```js
inertial : URDFInertialFrame
```

## URDFJoint

_extends THREE.Object3D_

### .jointType

```js
jointType = 'fixed' : string
```

### .axis

```js
axis : Vector3
```

### .limit

```js
limit : URDFLimit
```

## URDFRobot

_extends URDFLink_

### .robotName

```js
robotName = '' : string
```

## URDFVisual

_extends THREE.Object3D_

## URDFCollider

_extends THREE.Object3D_
