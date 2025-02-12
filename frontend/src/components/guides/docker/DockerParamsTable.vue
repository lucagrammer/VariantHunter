<!--

  Component:    DockerParamsTable
  Description:  Table to explain and list all the docker params

  Props:
  └── value: Value variable for binding of the flag to show the all the params

-->

<template>
  <v-simple-table class='mt-3 mb-4 table-with-borders'>
    <thead>
    <tr>
      <th class='text-left col-auto'>Parameter name</th>
      <th class='text-left col-auto'>Description</th>
      <th class='text-left col-auto'>Required</th>
    </tr>
    </thead>
    <tbody>

    <tr>
      <td>FILE_PATH</td>
      <td>
        <p>Path to the <span class='monospaced'>.tsv</span> metadata file.</p>
        Example: <span class='pl-3 monospaced'>FILE_PATH=/Users/rossi/datasets/metadata.tsv</span><br/>
        Example: <span class='pl-3 monospaced'>FILE_PATH=./metadata.tsv</span>
      </td>
      <td><b>Required</b>, unless importing an existing database.</td>
    </tr>

    <tr>
      <td>FILE_TYPE</td>
      <td>
        Type of the <span class='monospaced'>.tsv</span> metadata file. Currently supported:
        <ul>
          <li><span class='monospaced'>GISAID</span>: see <a href='#prerequisites'>prerequisites</a> for more
            information
          </li>
          <li><span class='monospaced'>NEXTSTRAIN</span>: see <a href='#prerequisites'>prerequisites</a> for more
            information
          </li>
        </ul>
        <p/>
        Example: <span class='pl-3 monospaced'>FILE_TYPE=GISAID</span>
      </td>
      <td><b>Optional</b>. <br/>Default: <span class='monospaced'>GISAID</span></td>
    </tr>

    <tr>
      <td>LOCATIONS</td>
      <td>
        <p>Comma separated list of country names whose sequence data is to be imported in the tool.<br/>
          Use "<span class='monospaced'>All</span>" to import the entire dataset.</p>
        Example: <span class='pl-3 monospaced'>LOCATIONS="Italy"</span><br/>
        Example: <span class='pl-3 monospaced'>LOCATIONS="Italy,Germany,Iran"</span>
      </td>
      <td><b>Optional</b>. <br/>Default: <span class='monospaced'>"All"</span></td>
    </tr>

    <tr>
      <td>START_DATE</td>
      <td>
        <p>Start date to be considered when importing data. <br/>
          Only the data in the period <span class='monospaced'>[START_DATE, END_DATE]</span> will be imported
          into the tool.</p>
        Example: <span class='pl-3 monospaced'>START_DATE=2021-12-01</span><br/>
      </td>
      <td><b>Optional</b>. <br/>Default: <span class='monospaced'>Beginning</span></td>
    </tr>

    <tr>
      <td>END_DATE</td>
      <td>
        <p>End date to be considered when importing data. <br/>
          Only the data in the period <span class='monospaced'>[START_DATE, END_DATE]</span> will be imported
          into the tool.</p>
        Example: <span class='pl-3 monospaced'>END_DATE=2021-12-30</span><br/>
      </td>
      <td><b>Optional</b>. <br/>Default: <span class='monospaced'>End</span></td>
    </tr>

    <tr v-if='!showAllParams' class='expand-table'>
      <td colspan='3'>
        <v-chip small @click.native='showAllParams=true'>
          <v-icon small left color='text_var1'>mdi-plus</v-icon>
          Show other optional advanced params
        </v-chip>
      </td>
    </tr>

    <tr v-if='showAllParams'>
      <td>PORT</td>
      <td>
        <p>The port to be used by the server</p>
        Example: <span class='pl-3 monospaced'>PORT=5001</span><br/>
      </td>
      <td><b>Optional</b>. <br/>Default: <span class='monospaced'>5000</span></td>
    </tr>

    <tr v-if='showAllParams'>
      <td>REGENERATE</td>
      <td>
        <p>Flag to drop the current database (if existing) and regenerate the data from the <span class='monospaced'>.tsv</span>
          file. </p>
        Example &nbsp; <i>[ overwrite an existing database ]</i>:
        <div class='pl-3 monospaced'>FILE_PATH=./metadata.tsv DB_PATH=./folder_to_overwrite REGENERATE=true</div>
      </td>
      <td><b>Optional</b>. <br/>Default: <span class='monospaced'>false</span></td>
    </tr>

    <tr v-if='showAllParams'>
      <td>DB_PATH</td>
      <td>
        <p>Path to the <span class='monospaced'>varianthunter.db</span> database file or location where to
          generate it. <br/>Further information about the structure of the
          <span class='monospaced'>.db</span> file can be found in the
          <a href='https://www.sqlite.org/fileformat.html#the_database_file' target='_blank'>SQLite documentation</a>.
        </p>
        Usage:
        <ul>
          <li>export the database file</li>
          <li>use a database file resulting from a previous execution, avoiding
            its regeneration starting from the <span class='monospaced'>.tsv</span> file.
          </li>
        </ul>
        <p/>
        <p><u>Notice</u>: if a database already exists and <span class='monospaced'>REGENERATE=true</span> is not
          specified, then all the params are ignored and the specified database is loaded.
        </p>
        Example: <span class='pl-3 monospaced'>DB_PATH=/Users/rossi/save_db_here</span><br/>
        Example: <span class='pl-3 monospaced'>DB_PATH=/Users/rossi/fetch_existing_db_from_here</span><br/>
      </td>
      <td><b>Optional</b>. <br/>Default: <span class='monospaced'>internal path</span></td>
    </tr>

    <tr v-if='showAllParams' class='expand-table'>
      <td colspan='3'>
        <v-chip small @click.native='showAllParams=false'>
          <v-icon small left color='text_var1'>mdi-minus</v-icon>
          Hide other optional advanced params
        </v-chip>
      </td>
    </tr>

    </tbody>
  </v-simple-table>
</template>

<script>
export default {
  name: 'DockerParamsTable',

  props: {
    /** Value variable for binding of the value */
    value: {}
  },

  computed: {
    /** Flag to show/hide the advanced optional params */
    showAllParams: {
      get() {
        return this.value
      },
      set(val) {
        this.$emit('input', val)
      }
    }
  }
}
</script>

<style scoped>

/* Table styling */
tr td:first-child, .monospaced {
  font-style: normal !important;
  font-family: monospace !important;
}

.table-with-borders {
  border: thin solid rgba(0, 0, 0, 0.12) !important;
  border-radius: var(--border-radius) !important;
}

tbody tr td {
  padding-top: 14px !important;
  padding-bottom: 14px !important;
  font-size: 14px !important;
}

tbody tr td:first-child {
  font-weight: bold;
}

</style>
