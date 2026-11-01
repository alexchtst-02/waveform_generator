# Waveform Generator

Use `waveform_generator.ipynb` to load a VCD, inspect its signals, and render selected waveforms. Run the notebook with `just_for_example/waveform_generator` as the working directory.

## Examples

Generate the AXI example VCD first, from `just_for_example`:

```bash
iverilog -g2012 -Wall -s tb_axi_read -o tb_axi_read.vvp axi_read.v tb_axi_read.v
vvp tb_axi_read.vvp
```

Open the notebook and run its cells in order. The first example loads the full VCD, lists signal names and widths, then previews all signals under `tb_axi_read`:

```python
data, all_signals, figure, axes = load_all_to_preview(
    Path("../axi_read_tb.vcd"),
    scope="tb_axi_read",
    include_internal=True,
    show_values=False,
)
```

To render only selected ports and hide bus labels:

```python
signals = [
    "tb_axi_read.m_axi_arvalid",
    "tb_axi_read.m_axi_arready",
    "tb_axi_read.m_axi_rdata",
]
figure, axes = plot_named_signals(
    data,
    signals,
    show_values={"tb_axi_read.m_axi_rdata": False},
)
```

Limit the figure to a time window in nanoseconds:

```python
figure, axes = plot_time_range(data, signals, start_ns=80, end_ns=320)
```

Add vertical time markers or shade intervals with the fixed mint color:

```python
figure, axes = plot_with_markers(data, signals, markers=[90, 195])
figure, axes = plot_with_shading(
    data,
    signals,
    intervals=[(90, 170), (240, 320)],
    shade_level=0.78,
)
```

`shade_level` ranges from `0.0` (transparent) to `1.0` (solid). `formats` accepts `bin`, `oct`, `dec`, or `hex`, with optional signed two's-complement display; `enums` maps integer values to names.
