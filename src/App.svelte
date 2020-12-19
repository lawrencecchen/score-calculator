<script>
  import { stateNames } from "./stateNames";
  import { cutoffs } from "./cutoffs";

  let readingWritingScore;
  let mathScore;
  let state = stateNames[0];
  const min = 160;
  const max = 760;
  $: semifinalistCutoff = cutoffs[state];
  $: tableYears = Object.keys(semifinalistCutoff);
  $: tableScores = Object.entries(semifinalistCutoff).map(
    ([year, score]) => score
  );
  $: console.log(state, semifinalistCutoff, tableYears, tableScores);

  function inRange(number, min, max) {
    return number <= max && number >= min;
  }

  $: error =
    readingWritingScore % 10 !== 0 ||
    !inRange(readingWritingScore, min, max) ||
    mathScore % 10 !== 0 ||
    !inRange(mathScore, min, max);
  $: selectionIndex = (readingWritingScore / 10) * 2 + mathScore / 10 || null;
</script>

<style global>
  @import "tailwindcss/base";

  @import "tailwindcss/components";

  @import "tailwindcss/utilities";
</style>

<div class="p-2 mx-auto w-full sm:max-w-xl">
  <h1 class="text-3xl font-bold text-black leading-8">
    PSAT National Merit Cutoff Calculator
  </h1>
  <div>
    <h1 class="text-xl font-bold text-black leading-8 mt-4">
      NMSC Selection Index (SI) Score Calculator
    </h1>
    <div class="text-xs text-gray-800 font-semibold">
      Both scores range from 160 to 760. (<a
        target="_blank"
        rel="noopener noreferrer"
        href="https://collegereadiness.collegeboard.org/pdf/sample-psat-nmsqt-score-report.pdf"
        class="text-indigo-600 active:text-indigo-300 underline">Need help?</a>)
    </div>
    <div class="w-full sm:max-w-md space-y-2 mt-2">
      <label class="block">
        <div class="text-gray-800">
          Evidence-Based Reading and Writing Score:
        </div>
        <input
          bind:value={readingWritingScore}
          type="number"
          placeholder="160-760"
          min="160"
          max="760"
          step="10"
          class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-300 focus:ring focus:ring-indigo-200 focus:ring-opacity-50" />
      </label>
      <label class="block">
        <div class="text-gray-800">Math Score:</div>
        <input
          bind:value={mathScore}
          type="number"
          placeholder="160-760"
          min="160"
          max="760"
          step="10"
          class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-300 focus:ring focus:ring-indigo-200 focus:ring-opacity-50" />
      </label>
    </div>
  </div>

  <div>
    {#if selectionIndex && !error}
      <div class="text-xl font-bold text-black leading-8 mt-4">
        Your NMSC Selection Index:
      </div>
      <div class="text-xs font-semibold text-gray-800">
        The NMSC Selection Index ranges from 48 to 228
      </div>
      <div class="text-xl space-y-2">
        <div class="tabular-nums flex flex-row items-center">
          <div class="font-mono slashed-zero">
            {mathScore}
            + ({readingWritingScore}
            × 2) =
          </div>
          <span
            class="font-black text-3xl p-1 bg-indigo-200 ml-4">{selectionIndex}</span>
        </div>
      </div>
    {:else}
      <div class="text-red-600 font-semibold">Please enter valid scores.</div>
    {/if}
  </div>

  <div>
    <h1 class="text-xl font-bold text-black leading-8 mt-4">
      Semifinalist Qualification Cutoffs
    </h1>
    <div class="w-full sm:max-w-md space-y-2 mt-2">
      <label class="block">
        <div class="text-gray-800">Your Home State:</div>
        <select
          bind:value={state}
          type="dropdown"
          placeholder="160-760"
          min="160"
          max="760"
          step="10"
          class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-300 focus:ring focus:ring-indigo-200 focus:ring-opacity-50">
          {#each stateNames as state}
            <option value={state}>{state}</option>
          {/each}
        </select>
      </label>

      {#if state}
        <div>Past data for {state}</div>
        <table
          class="table-auto border-collapse border border-gray-300 rounded-md shadow-sm">
          <!-- <thead>
            <tr>
              <th>State</th>
            </tr>
          </thead> -->
          <tbody>
            <tr>
              <td class="border border-gray-300 px-2 py-3 font-bold bg-gray-50">
                Class of
              </td>
              {#each tableYears as year}
                <td
                  class="border border-gray-300 px-2 py-3 text-center tabular-nums">
                  {year}
                </td>
              {/each}
            </tr>
            <tr>
              <td class="border border-gray-300 px-2 py-3 font-bold bg-gray-50">
                Semifinalist SI cutoff
              </td>
              {#each tableScores as score}
                <td
                  class="border border-gray-300 px-2 py-3 text-center tabular-nums">
                  {score}
                </td>
              {/each}
            </tr>
          </tbody>
        </table>
      {/if}
    </div>
  </div>
</div>
