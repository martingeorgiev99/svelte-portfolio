<script>
  import { Link } from 'svelte-routing';
</script>

<style>
  nav {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 70px;
    background-color: #2b2a2a;
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 0 20px;
    z-index: 1000;
  }

  ul {
    display: flex;
    justify-content: flex-end;
    align-items: center;
    list-style: none;
    margin-left: auto;
    padding-right: 50px;
  }

  li {
    margin-left: 20px;
    color: white;
  }
</style>

<nav>
  <ul>
    <li><Link to="/">Home</Link></li>
    <li><Link to="/education">Education</Link></li>
    <li><Link to="/experience">Experience</Link></li>
    <li><Link to="/projects">Projects</Link></li>
    <li><Link to="/skills">Skills</Link></li>
  </ul>
</nav>
