# Taming 3DGS: High-Quality Radiance Fields with Limited Resources
Saswat Subhajyoti Mallick*, Rahul Goel*, Bernhard Kerbl, Francisco Vicente Carrasco, Markus Steinberger, Fernando De La Torre (* indicates equal contribution)



**TL;DR** We improve the densification process to make the primitive count deterministic and implement several low-level optimizations for fast convergence.



<details>
<summary><span style="font-weight: bold;">additional flags for train.py</span></summary>

  #### --cams
  Number of cameras required to compute gaussian scores. Default set to 10.
  #### --budget
  The final number of gaussians to end up with. Can be a float or an integer based on `--mode`.
  #### --mode
  multiplier: the final count of gaussians will be `multiplier` x the initial (SfM) count <br>
  final_count: the final count of gaussians will be set exactly to `final_count`.
  #### --websockets
  Whether to use the web based viewer or not.
  #### --ho_iteration
  High opacity gaussians will be enabled from which iteration. Defaults to 15000 (after densification ends).
  #### --sh_lower
  Whether to enable less-frequent (once every 16 iterations) SH updates to gain speed.
  #### --benchmark_dir
  The location of the folder where the timing results are stored. No time profiling will be done if left blank.
</details>
<br>

## Reproducibility

Our experiments were carried out on a machine with 24 vCPUs, 512GB RAM and 1xNvidia RTX A4500 20G GPU. For reproducing the results reported in our paper, please run the following scripts from the project root:
```bash
# MODE={budget|big}
python full_eval.py \
    -m360 <MipNeRF360 dataset path> \
    -tat <TanksAndTemples dataset path> \
    -db <DeepBlending dataset path> \
    --sh_lower \
    --mode ${MODE}
```

* `MODE=budget`: results on budgeted setting [reported as **Ours** in the paper]
* `MODE=big`   : results on unconstrained setting [reported as **Ours(Big)** in the paper]

Add `--dry_run` to print the commands without executing them. The `train.sh` has a compiled list of scripts for evaluation.

## Web viewer
We provide a basic browser-based renderer to track the training progress. Steps for usage are as follows: <br>
1. Move the [web-viewer](./web_viewer/) folder into the host machine from where you wish to view the training.
2. Assign a port number while spawning a training session using `train.py --port <port_num> --websockets`. If you're running this on a remote server, forward the port to your host system.
3. Maintain the same port number in the [app.js](./web_viewer/app.js) file.
4. Open [render.html](./web_viewer/render.html) in your browser.





```
