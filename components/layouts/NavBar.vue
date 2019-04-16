<template>
  <div>
    <b-navbar type="dark" variant="dark">
      <b-navbar-nav>
        <b-nav-item href="#">Ruby Online</b-nav-item>

        <b-nav-item-dropdown text="Menu" right>
          <b-dropdown-item to="/about">About</b-dropdown-item>
          <b-dropdown-item to="/contact">Contact</b-dropdown-item>
          <b-dropdown-item to="/code">Code</b-dropdown-item>
        </b-nav-item-dropdown>

      </b-navbar-nav>
    </b-navbar>
  </div>
</template>
