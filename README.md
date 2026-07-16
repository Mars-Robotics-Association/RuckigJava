# RuckigJava

Pure-Java port of the [Ruckig](https://github.com/pantor/ruckig) Community online trajectory
generation library (MIT license).

Drop this repo into any Java project (Gradle composite build, git submodule, or copy the
`src/main/java` tree). It has no runtime dependencies beyond the JDK.

See [PORTING_LOG.md](PORTING_LOG.md) for file-by-file mapping and [DIVERGENCES.md](DIVERGENCES.md)
for intentional/known differences (expected empty for a faithful transliteration).

## Module rules

- **Java 8** source/target compatibility.
- **Zero dependencies** beyond the JDK at runtime.
- **Transliteration, not reimplementation** — names, control flow, and FP operation order
  match upstream so golden-vector tests stay meaningful.
- Steady-state `Ruckig.update` is designed for zero allocation (preallocated profiles/blocks).

## Build & test

```bash
./gradlew test
```

## Golden vectors

Generators live under `tools/`:

- `tools/generate_golden.py` — preferred when the upstream Python package is available
- `tools/generate_golden_cpp.cpp` — offline C++ harness against vendored Ruckig headers

Checked-in goldens: `src/test/resources/golden/`.

## Public API (Community)

```java
Ruckig otg = new Ruckig(1, /*delta_time*/ 0.01);
InputParameter input = new InputParameter(1);
OutputParameter output = new OutputParameter(1);

input.current_position[0] = 0;
input.target_position[0] = 1;
input.max_velocity[0] = 2;
input.max_acceleration[0] = 2;
input.max_jerk[0] = 4;

int result = otg.update(input, output);
// output.new_position / new_velocity / new_acceleration
```

## License

MIT — see [LICENSE](LICENSE) (upstream copyright retained).
