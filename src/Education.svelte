<style>
  main {
    padding: 20px;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    height: 100vh;
  }

  .education-header {
    display: flex;
    align-items: center;
    justify-content: center;
    margin-bottom: 20px;
  }

  .animated-image {
    width: 200px;
    height: auto;
    margin-right: 20px;
    animation: yourAnimation 1s infinite;
    border-radius: 50%;
  }

  .education-text {
    flex-grow: 1;
    text-align: center;
    color: white;
  }

  h1 {
    color: white;
  }

  h1 span {
    color: purple;
  }

  .timeline {
    display: flex;
    flex-direction: column;
    align-items: center;
    margin-top: 50px;
  }

  .timeline-item {
    text-align: center;
    margin-bottom: 20px;
    color: white;
  }

  h2.education-text span {
    color: purple;
  }
</style>

<main>
  <div class="education-header">
    <img class="animated-image" src="/images/moon.gif" alt="Animated Waves" />
    <div class="education-text">
      <h1>Educational <span>Background</span></h1>
      <p>“A person who won’t read has no advantage over a person who can’t read.” — Mark Twain</p>
    </div>
  </div>

  <div class="timeline">
    <h2 class="education-text">Formal <span>Education</span></h2>
    <div class="timeline-item">
      <h3>HZ UAS</h3>
      <p>Degree in ICT, Data Science Specialisation</p>
      <p>Year: 2022 - 2026</p>
    </div>
    <div class="timeline-item">
      <h3>Sofia Vocational High School of Electronics John Atanasoff</h3>
      <p>High School Diploma</p>
      <p>Year: 2015 - 2022</p>
    </div>
  </div>
</main>
