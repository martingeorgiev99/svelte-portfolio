<script>
  import { Router, Route } from 'svelte-routing';
  import Navbar from './Navbar.svelte';
  import Home from './Home.svelte';
  import Education from './Education.svelte';
  import Experience from './Experience.svelte';
  import Projects from './Projects.svelte';
  import Skills from './Skills.svelte';
  import './global.css';
</script>

<Router>
  <main>
    <Navbar />
    <Route path="/" component={Home} />
    <Route path="/education" component={Education} />
    <Route path="/experience" component={Experience} />
    <Route path="/projects" component={Projects} />
    <Route path="/skills" component={Skills} />
  </main>
</Router>
