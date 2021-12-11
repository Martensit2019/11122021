<template>
  <div class="minh">
    <NuxtLink v-if="lecture.data.prev" :to="toLink(lecture.data.prev)">
      <arrowPrev v-if="lecture.data.prev" />
    </NuxtLink>
    <NuxtLink v-if="lecture.data.next" :to="toLink(lecture.data.next)">
      <arrowNext v-if="lecture.data.next" />
    </NuxtLink>
    <div class="breadcrumb">
      <NuxtLink
        :to="{
          name: isCourse ? 'courses-course' : 'programs-program_id',
          params: isCourse
            ? { course: this.$route.params.course }
            : { program_id: $route.params.program_id },
        }"
        class="breadcrumb__item mr_2"
      >
        <span class="material-icons-outlined btn btn-back btn-non-focus mr_2">
          arrow_back_ios
        </span>
        <div v-if="isCourse" class="breadcrumb__title">
          <span class="breadcrumb__label">Курс:</span>
          {{ lecture.data.course.name }}
        </div>
        <div v-else class="breadcrumb__title">
          <span class="breadcrumb__label">Программа:</span>
          {{ lecture.data.education_program.name }}
        </div>
      </NuxtLink>
      <NuxtLink
        v-if="!isCourse"
        :to="{
          name: 'programs-program_id-course_id',
          params: {
            program_id: $route.params.program_id,
            course_id: $route.params.course_id,
          },
        }"
        class="breadcrumb__item mr_2"
      >
        <span class="material-icons-outlined btn btn-back btn-non-focus mr_2">
          arrow_back_ios
        </span>
        <div class="breadcrumb__title">
          <span class="breadcrumb__label">Курс:</span>
          {{ lecture.data.course.name }}
        </div>
      </NuxtLink>
    </div>
    <div class="program-index_inner_content lectures-header mt_3 mb_4">
      <div class="desktop__name_section">лекция</div>
      <div class="lectures-name">
        {{ lecture.data.lesson.name }}
      </div>
    </div>
    <div v-if="lecture.data.lesson.video" class="lectures-slider">
      <div class="lectures-slider_wrapper">
        <client-only>
          <div class="vjs-theme-lecture">
            <VideoPlayer :options="videoOptions" />
          </div>
        </client-only>
        <div v-if="false" class="lectures-votes mt_2 mb_5">
          <div class="lectures-votes__likes mr_1">
            <svg
              width="24"
              height="24"
              viewBox="0 0 24 24"
              fill="none"
              xmlns="http://www.w3.org/2000/svg"
            >
              <path
                d="M23.217 12.6334C23.721 11.9869 24 11.1948 24 10.3706C24 9.06299 23.247 7.82528 22.035 7.13507C21.723 6.95741 21.3677 6.8639 21.006 6.86423H13.812L13.992 3.28506C14.034 2.42011 13.719 1.59886 13.107 0.972718C12.8067 0.6641 12.4443 0.418552 12.0423 0.251249C11.6404 0.0839456 11.2073 -0.0015581 10.77 2.1493e-05C9.21 2.1493e-05 7.83 1.01931 7.416 2.47836L4.839 11.5355H0.96C0.429 11.5355 0 11.952 0 12.4674V23.0681C0 23.5835 0.429 24 0.96 24H18.999C19.275 24 19.545 23.9476 19.794 23.8427C21.222 23.2515 22.143 21.8973 22.143 20.3946C22.143 20.0277 22.089 19.6665 21.981 19.3171C22.485 18.6706 22.764 17.8784 22.764 17.0542C22.764 16.6873 22.71 16.3262 22.602 15.9767C23.106 15.3302 23.385 14.5381 23.385 13.7139C23.379 13.3469 23.325 12.9829 23.217 12.6334ZM2.16 21.9032V13.6323H4.59V21.9032H2.16ZM21.252 11.6229L20.595 12.1762L21.012 12.9159C21.1494 13.1596 21.2206 13.4332 21.219 13.711C21.219 14.1915 21.003 14.6487 20.631 14.9632L19.974 15.5166L20.391 16.2563C20.5284 16.5 20.5996 16.7736 20.598 17.0513C20.598 17.5319 20.382 17.9891 20.01 18.3036L19.353 18.8569L19.77 19.5967C19.9074 19.8403 19.9786 20.1139 19.977 20.3917C19.977 21.0441 19.581 21.6323 18.969 21.9003H6.51V13.5391L9.495 3.04043C9.57197 2.77133 9.7372 2.53393 9.96565 2.36419C10.1941 2.19445 10.4733 2.10162 10.761 2.09976C10.989 2.09976 11.214 2.16383 11.394 2.29489C11.691 2.51039 11.85 2.83657 11.832 3.18313L11.544 8.96106H20.976C21.51 9.2785 21.84 9.81436 21.84 10.3706C21.84 10.8511 21.624 11.3054 21.252 11.6229Z"
                fill="#B2B2B2"
              />
            </svg>
            12
          </div>
          <div class="lectures-votes__dislikes">
            <svg
              width="24"
              height="24"
              viewBox="0 0 24 24"
              fill="none"
              xmlns="http://www.w3.org/2000/svg"
            >
              <path
                d="M0.782999 11.3664C0.674999 11.0169 0.620998 10.6557 0.620998 10.2886C0.620998 9.46427 0.9 8.67194 1.404 8.02526C1.296 7.6757 1.242 7.3145 1.242 6.94746C1.242 6.12309 1.521 5.33076 2.025 4.68408C1.917 4.33453 1.863 3.97332 1.863 3.60628C1.863 2.10319 2.784 0.748658 4.212 0.157325C4.463 0.0522497 4.73367 -0.0013046 5.007 2.41328e-05H23.04C23.571 2.41328e-05 24 0.416579 24 0.932175V11.5354C24 12.051 23.571 12.4675 23.04 12.4675H19.161L16.587 21.5211C16.173 22.9805 14.793 24 13.233 24C12.342 24 11.511 23.6563 10.896 23.0271C10.281 22.4008 9.966 21.5793 10.011 20.7142L10.191 17.1341H2.994C2.631 17.1341 2.277 17.0409 1.965 16.8632C0.752998 16.1787 -1.90735e-06 14.9377 -1.90735e-06 13.6298C-1.90735e-06 12.8054 0.278999 12.0131 0.782999 11.3664ZM21.84 10.3673V2.09445H19.41V10.3673H21.84ZM3.024 15.0397H12.456L12.168 20.819C12.15 21.1657 12.309 21.4919 12.606 21.7075C12.789 21.8386 13.014 21.9056 13.239 21.9027C13.5265 21.9 13.8052 21.8068 14.0335 21.6371C14.2618 21.4675 14.4272 21.2305 14.505 20.9618L17.49 10.4605V2.09445H5.028C4.72813 2.22494 4.47347 2.43677 4.29465 2.70446C4.11583 2.97215 4.02045 3.2843 4.02 3.60337C4.02 3.88593 4.089 4.15392 4.227 4.39861L4.644 5.13851L3.987 5.69197C3.80207 5.84749 3.65383 6.03987 3.55237 6.25602C3.4509 6.47217 3.39859 6.70701 3.399 6.94455C3.399 7.22711 3.468 7.4951 3.606 7.73979L4.023 8.47968L3.366 9.03315C3.18107 9.18867 3.03283 9.38105 2.93136 9.5972C2.8299 9.81334 2.77759 10.0482 2.778 10.2857C2.778 10.5683 2.847 10.8363 2.985 11.081L3.405 11.8238L2.748 12.3772C2.56307 12.5328 2.41483 12.7251 2.31337 12.9413C2.2119 13.1574 2.15959 13.3923 2.16 13.6298C2.16 14.1862 2.49 14.7222 3.024 15.0397Z"
                fill="#B2B2B2"
              />
            </svg>
            0
          </div>
        </div>
      </div>
    </div>
    <div class="mt_3 lectures-program lectures-slider_wrapper">
      <div v-html="lecture.data.lesson.content" />
      <div v-if="lecture.data.lesson.materials.length" class="mt_3">
        <h5><b>Дополнительные материалы</b></h5>
        <a
          v-for="(item, key) in lecture.data.lesson.materials"
          :key="key"
          :href="item.link"
        >
          {{ item.name }}
        </a>
      </div>
      <button
        v-if="!isComplete"
        type="button"
        class="btn btn__primary non-focus kim_auto-width mr_2 d-block mx-auto"
        @click="finishLesson"
      >
        Лекция пройдена
      </button>
    </div>
    <!-- ---------------------- -->
    <!-- <div v-for="(item, key) in listCourses" :key="key">
      <Lecture
        :key="key"
        :program="item"
        :desktop="true"
        :category-id="category.slug"
      />
    </div> -->
    <!-- -------------------- -->
    <div class="lectures_desktop">
      <!-- <div>
        <div class="program program_themes_desktop">        
          
        </div>
      </div> -->

      <div class="lectures_item">
        <div class="check check_visible">
          <svg
            width="14"
            height="11"
            viewBox="0 0 14 11"
            fill="none"
            xmlns="http://www.w3.org/2000/svg"
          >
            <path
              d="M11.8238 0.350702C12.0657 0.123273 12.3877 -0.00245916 12.7217 3.64524e-05C13.0557 0.00253207 13.3757 0.133061 13.6141 0.364078C13.8525 0.595096 13.9907 0.908535 13.9995 1.23825C14.0084 1.56797 13.8871 1.88819 13.6614 2.13132L6.809 10.5935C6.69117 10.7188 6.54895 10.8194 6.39086 10.8892C6.23277 10.959 6.06205 10.9966 5.8889 10.9998C5.71575 11.0029 5.54373 10.9716 5.38313 10.9076C5.22253 10.8436 5.07665 10.7483 4.9542 10.6274L0.409958 6.14026C0.283408 6.02382 0.181906 5.8834 0.111507 5.72739C0.0411069 5.57137 0.00325191 5.40295 0.000200455 5.23218C-0.002851 5.06141 0.0289636 4.89177 0.0937458 4.7334C0.158528 4.57503 0.254951 4.43117 0.377262 4.31039C0.499573 4.18962 0.645267 4.09441 0.805651 4.03044C0.966036 3.96647 1.13783 3.93506 1.31077 3.93807C1.48372 3.94108 1.65428 3.97846 1.81228 4.04798C1.97028 4.11749 2.11248 4.21772 2.2304 4.34268L5.82664 7.89205L11.7912 0.388011C11.8019 0.374954 11.8134 0.362498 11.8255 0.350702H11.8238Z"
              fill="white"
            />
          </svg>
        </div>
        <div class="lectures_item-wrapper lectures_item-wrapper--darkened">
          <div class="program__image"></div>
          <div class="lecture__info">
            <div class="program__title">
              Название лекции в несколько строк, в две, например
            </div>
            <div class="program__stats program_stats_mb">
              <div class="program__stats_like program_stats_mr">
                <span
                  class="
                    material-icons-outlined
                    program_stats_icon_mr program_like_red
                  "
                  >favorite</span
                >13
              </div>
              <div class="program__stats_views program_stats_mr">
                <span class="material-icons program_stats_icon_mr">people</span
                >225
              </div>
              <div class="program__stats_duration">
                <span class="material-icons program_stats_icon_mr"
                  >query_builder</span
                >1 час 23 мин
              </div>
            </div>
            <div class="program__tags">
              <div class="program__tag">#ТЭГ1</div>
              <div class="program__tag">#ТЭГ2</div>
              <div class="program__tag">#ТЭГ3</div>
              <div class="program__tag">#ТЭГ4</div>
            </div>
          </div>
        </div>
      </div>
      <div class="lectures_item">
        <div class="check check_visible">
          <svg
            width="14"
            height="11"
            viewBox="0 0 14 11"
            fill="none"
            xmlns="http://www.w3.org/2000/svg"
          >
            <path
              d="M11.8238 0.350702C12.0657 0.123273 12.3877 -0.00245916 12.7217 3.64524e-05C13.0557 0.00253207 13.3757 0.133061 13.6141 0.364078C13.8525 0.595096 13.9907 0.908535 13.9995 1.23825C14.0084 1.56797 13.8871 1.88819 13.6614 2.13132L6.809 10.5935C6.69117 10.7188 6.54895 10.8194 6.39086 10.8892C6.23277 10.959 6.06205 10.9966 5.8889 10.9998C5.71575 11.0029 5.54373 10.9716 5.38313 10.9076C5.22253 10.8436 5.07665 10.7483 4.9542 10.6274L0.409958 6.14026C0.283408 6.02382 0.181906 5.8834 0.111507 5.72739C0.0411069 5.57137 0.00325191 5.40295 0.000200455 5.23218C-0.002851 5.06141 0.0289636 4.89177 0.0937458 4.7334C0.158528 4.57503 0.254951 4.43117 0.377262 4.31039C0.499573 4.18962 0.645267 4.09441 0.805651 4.03044C0.966036 3.96647 1.13783 3.93506 1.31077 3.93807C1.48372 3.94108 1.65428 3.97846 1.81228 4.04798C1.97028 4.11749 2.11248 4.21772 2.2304 4.34268L5.82664 7.89205L11.7912 0.388011C11.8019 0.374954 11.8134 0.362498 11.8255 0.350702H11.8238Z"
              fill="white"
            />
          </svg>
        </div>
        <div class="lectures_item-wrapper lectures_item-wrapper--darkened">
          <div class="program__image"></div>
          <div class="lecture__info">
            <div class="program__title">
              Название лекции в несколько строк, в две, например
            </div>
            <div class="program__stats program_stats_mb">
              <div class="program__stats_like program_stats_mr">
                <span
                  class="
                    material-icons-outlined
                    program_stats_icon_mr program_like_red
                  "
                  >favorite</span
                >13
              </div>
              <div class="program__stats_views program_stats_mr">
                <span class="material-icons program_stats_icon_mr">people</span
                >225
              </div>
              <div class="program__stats_duration">
                <span class="material-icons program_stats_icon_mr"
                  >query_builder</span
                >1 час 23 мин
              </div>
            </div>
            <div class="program__tags">
              <div class="program__tag">#ТЭГ1</div>
              <div class="program__tag">#ТЭГ2</div>
              <div class="program__tag">#ТЭГ3</div>
              <div class="program__tag">#ТЭГ4</div>
            </div>
          </div>
        </div>
      </div>
      <div class="lectures_item">
        <div class="check check_visible">
          <svg
            width="14"
            height="11"
            viewBox="0 0 14 11"
            fill="none"
            xmlns="http://www.w3.org/2000/svg"
          >
            <path
              d="M11.8238 0.350702C12.0657 0.123273 12.3877 -0.00245916 12.7217 3.64524e-05C13.0557 0.00253207 13.3757 0.133061 13.6141 0.364078C13.8525 0.595096 13.9907 0.908535 13.9995 1.23825C14.0084 1.56797 13.8871 1.88819 13.6614 2.13132L6.809 10.5935C6.69117 10.7188 6.54895 10.8194 6.39086 10.8892C6.23277 10.959 6.06205 10.9966 5.8889 10.9998C5.71575 11.0029 5.54373 10.9716 5.38313 10.9076C5.22253 10.8436 5.07665 10.7483 4.9542 10.6274L0.409958 6.14026C0.283408 6.02382 0.181906 5.8834 0.111507 5.72739C0.0411069 5.57137 0.00325191 5.40295 0.000200455 5.23218C-0.002851 5.06141 0.0289636 4.89177 0.0937458 4.7334C0.158528 4.57503 0.254951 4.43117 0.377262 4.31039C0.499573 4.18962 0.645267 4.09441 0.805651 4.03044C0.966036 3.96647 1.13783 3.93506 1.31077 3.93807C1.48372 3.94108 1.65428 3.97846 1.81228 4.04798C1.97028 4.11749 2.11248 4.21772 2.2304 4.34268L5.82664 7.89205L11.7912 0.388011C11.8019 0.374954 11.8134 0.362498 11.8255 0.350702H11.8238Z"
              fill="white"
            />
          </svg>
        </div>
        <div class="lectures_item-wrapper lectures_item-wrapper--darkened">
          <div class="program__image"></div>
          <div class="lecture__info">
            <div class="program__title">
              Название лекции в несколько строк, в две, например
            </div>
            <div class="program__stats program_stats_mb">
              <div class="program__stats_like program_stats_mr">
                <span
                  class="
                    material-icons-outlined
                    program_stats_icon_mr program_like_red
                  "
                  >favorite</span
                >13
              </div>
              <div class="program__stats_views program_stats_mr">
                <span class="material-icons program_stats_icon_mr">people</span
                >225
              </div>
              <div class="program__stats_duration">
                <span class="material-icons program_stats_icon_mr"
                  >query_builder</span
                >1 час 23 мин
              </div>
            </div>
            <div class="program__tags">
              <div class="program__tag">#ТЭГ1</div>
              <div class="program__tag">#ТЭГ2</div>
              <div class="program__tag">#ТЭГ3</div>
              <div class="program__tag">#ТЭГ4</div>
            </div>
          </div>
        </div>
      </div>
      <div class="lectures_item">
        <div class="check">
          <svg
            width="14"
            height="11"
            viewBox="0 0 14 11"
            fill="none"
            xmlns="http://www.w3.org/2000/svg"
          >
            <path
              d="M11.8238 0.350702C12.0657 0.123273 12.3877 -0.00245916 12.7217 3.64524e-05C13.0557 0.00253207 13.3757 0.133061 13.6141 0.364078C13.8525 0.595096 13.9907 0.908535 13.9995 1.23825C14.0084 1.56797 13.8871 1.88819 13.6614 2.13132L6.809 10.5935C6.69117 10.7188 6.54895 10.8194 6.39086 10.8892C6.23277 10.959 6.06205 10.9966 5.8889 10.9998C5.71575 11.0029 5.54373 10.9716 5.38313 10.9076C5.22253 10.8436 5.07665 10.7483 4.9542 10.6274L0.409958 6.14026C0.283408 6.02382 0.181906 5.8834 0.111507 5.72739C0.0411069 5.57137 0.00325191 5.40295 0.000200455 5.23218C-0.002851 5.06141 0.0289636 4.89177 0.0937458 4.7334C0.158528 4.57503 0.254951 4.43117 0.377262 4.31039C0.499573 4.18962 0.645267 4.09441 0.805651 4.03044C0.966036 3.96647 1.13783 3.93506 1.31077 3.93807C1.48372 3.94108 1.65428 3.97846 1.81228 4.04798C1.97028 4.11749 2.11248 4.21772 2.2304 4.34268L5.82664 7.89205L11.7912 0.388011C11.8019 0.374954 11.8134 0.362498 11.8255 0.350702H11.8238Z"
              fill="white"
            />
          </svg>
        </div>
        <div class="lectures_item-wrapper">
          <div class="program__image"></div>
          <div class="lecture__info">
            <div class="program__title">
              Название лекции в несколько строк, в две, например
            </div>
            <div class="program__stats program_stats_mb">
              <div class="program__stats_like program_stats_mr">
                <span
                  class="
                    material-icons-outlined
                    program_stats_icon_mr program_like_red
                  "
                  >favorite</span
                >13
              </div>
              <div class="program__stats_views program_stats_mr">
                <span class="material-icons program_stats_icon_mr">people</span
                >225
              </div>
              <div class="program__stats_duration">
                <span class="material-icons program_stats_icon_mr"
                  >query_builder</span
                >1 час 23 мин
              </div>
            </div>
            <div class="program__tags">
              <div class="program__tag">#ТЭГ1</div>
              <div class="program__tag">#ТЭГ2</div>
              <div class="program__tag">#ТЭГ3</div>
              <div class="program__tag">#ТЭГ4</div>
            </div>
          </div>
        </div>
      </div>
      <div class="lectures_item">
        <div class="check">
          <svg
            width="14"
            height="11"
            viewBox="0 0 14 11"
            fill="none"
            xmlns="http://www.w3.org/2000/svg"
          >
            <path
              d="M11.8238 0.350702C12.0657 0.123273 12.3877 -0.00245916 12.7217 3.64524e-05C13.0557 0.00253207 13.3757 0.133061 13.6141 0.364078C13.8525 0.595096 13.9907 0.908535 13.9995 1.23825C14.0084 1.56797 13.8871 1.88819 13.6614 2.13132L6.809 10.5935C6.69117 10.7188 6.54895 10.8194 6.39086 10.8892C6.23277 10.959 6.06205 10.9966 5.8889 10.9998C5.71575 11.0029 5.54373 10.9716 5.38313 10.9076C5.22253 10.8436 5.07665 10.7483 4.9542 10.6274L0.409958 6.14026C0.283408 6.02382 0.181906 5.8834 0.111507 5.72739C0.0411069 5.57137 0.00325191 5.40295 0.000200455 5.23218C-0.002851 5.06141 0.0289636 4.89177 0.0937458 4.7334C0.158528 4.57503 0.254951 4.43117 0.377262 4.31039C0.499573 4.18962 0.645267 4.09441 0.805651 4.03044C0.966036 3.96647 1.13783 3.93506 1.31077 3.93807C1.48372 3.94108 1.65428 3.97846 1.81228 4.04798C1.97028 4.11749 2.11248 4.21772 2.2304 4.34268L5.82664 7.89205L11.7912 0.388011C11.8019 0.374954 11.8134 0.362498 11.8255 0.350702H11.8238Z"
              fill="white"
            />
          </svg>
        </div>
        <div class="lectures_item-wrapper">
          <div class="program__image"></div>
          <div class="lecture__info">
            <div class="program__title">
              Название лекции в несколько строк, в две, например
            </div>
            <div class="program__stats program_stats_mb">
              <div class="program__stats_like program_stats_mr">
                <span
                  class="
                    material-icons-outlined
                    program_stats_icon_mr program_like_red
                  "
                  >favorite</span
                >13
              </div>
              <div class="program__stats_views program_stats_mr">
                <span class="material-icons program_stats_icon_mr">people</span
                >225
              </div>
              <div class="program__stats_duration">
                <span class="material-icons program_stats_icon_mr"
                  >query_builder</span
                >1 час 23 мин
              </div>
            </div>
            <div class="program__tags">
              <div class="program__tag">#ТЭГ1</div>
              <div class="program__tag">#ТЭГ2</div>
              <div class="program__tag">#ТЭГ3</div>
              <div class="program__tag">#ТЭГ4</div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import ArrowPrev from "~/components/programs/blocks/ArrowPrev";
import ArrowNext from "~/components/programs/blocks/ArrowNext";
import VideoPlayer from "~/components/common/VideoPlayer";
export default {
  name: "lecture",
  components: { VideoPlayer, ArrowPrev, ArrowNext },
  props: {
    lecture: {
      type: Object,
      default: {},
    },
    isCourse: {
      type: Boolean,
      default: false,
    },
  },
  data() {
    return {
      links: 2,
      hasVideo: true,
      videoOptions: {
        language: "ru",
        controls: true,
        preload: "auto",
        sources: [
          {
            src: "",
            type: "video/mp4",
          },
        ],
        playbackRates: [1, 1.25, 1.5, 1.75, 2],
        fluid: true,
      },
    };
  },
  head() {
    return {
      title: this.lecture.data.lesson.name || "Название",
    };
  },
  computed: {
    isComplete() {
      return this.lecture.data.lesson.is_completed;
    },
  },
  mounted() {
    this.videoOptions.sources[0].src = this.lecture.data.lesson.video + "#t=1";
    this.$nextTick(() => {
      setTimeout(() => {
        this.$store.commit("categories/SET_LOADING", false);
      }, 500);
    });
  },
  methods: {
    toLink(item) {
      if (this.isCourse) {
        const name =
          item.type === "kim" ? "courses-course-kim" : "courses-course-lecture";
        const params =
          item.type === "kim"
            ? {
                course: this.$route.params.course,
                kim: item.id,
              }
            : {
                course: this.$route.params.course,
                lecture: item.slug,
              };
        return {
          name,
          params,
        };
      } else {
        const name =
          item.type === "kim"
            ? "programs-program_id-course_id-test-kim_id"
            : "programs-program_id-course_id-lecture_id";
        const params =
          item.type === "kim"
            ? {
                program_id: this.$route.params.program_id,
                course_id: this.$route.params.course_id,
                kim_id: item.id,
              }
            : {
                program_id: this.$route.params.program_id,
                course_id: this.$route.params.course_id,
                lecture_id: item.slug,
              };
        return {
          name,
          params,
        };
      }
    },
    finishLesson() {
      const data = this.lecture.data;
      if (this.isCourse) {
        this.$axios
          .post(
            `/course/${data.course.slug}/lesson/${data.lesson.slug}/complete`
          )
          .then(() => {
            let route = `/courses/${this.$route.params.course}`;
            if (this.lecture.data.course.hasOwnProperty("next_item")) {
              route += `/${data.course.next_item.slug}`;
            }
            this.$router.push(route);
          });
      } else {
        this.$axios
          .post(
            `/programs/${data.education_program.id}/course/${data.course.id}/lesson/${data.lesson.id}/complete`
          )
          .then(
            this.$router.push(
              `/programs/${this.$route.params.program_id}/${this.$route.params.course_id}`
            )
          );
      }
    },
  },
};
</script>

<style scope module>
img {
  width: auto;
  max-width: 1100px;
  height: auto;
  object-fit: contain;
  display: block;
}
.btn__primary-lecture {
  color: red;
}
</style>

<style>
[v-cloak] {
  display: none;
}
.minh {
  min-height: calc(100vh - 336px);
}
.lectures_desktop {
  margin: 32px auto;
  max-width: 1113px;
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-gap: 30px;
  margin-right: 100px;
  position: relative;
}
.lectures_item-wrapper--darkened {
  opacity: 0.2;
}
.program__image {
  position: relative;
  background-image: url("~/assets/images/programs/lecture-bg.jpg");
  height: 197px;
}
.program__image:hover{
  border: 6px solid #0057FF;
  cursor: pointer;
}
.lectures_item {
  max-width: 351px;
  position: relative;
}
.lectures_item::before {
  display: block;
  position: absolute;

  z-index: 99;
}

.check {
  display: none;
  position: absolute;
  width: 40px;
  height: 40px;
  border-radius: 50%;
  top: 79px;
  left: 50%;
  transform: translateX(-50%);
  background-color: #40bf54;
  z-index: 100;
}
.check_visible {
  display: flex;
  justify-content: center;
  align-items: center;
}
.lecture__info {
    padding: 20px 0;
    background-color: white;
}
</style>
